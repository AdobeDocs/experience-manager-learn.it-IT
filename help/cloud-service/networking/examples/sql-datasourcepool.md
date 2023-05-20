---
title: Connessioni SQL con DataSourcePool JDBC
description: Scopri come connettersi ai database SQL da AEM as a Cloud Service utilizzando il DataSourcePool JDBC AEM e le porte di uscita.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Connessioni SQL con DataSourcePool JDBC

Le connessioni ai database SQL (e ad altri servizi non HTTP/HTTPS) devono essere escluse dall’AEM, incluse quelle effettuate utilizzando il servizio OSGi DataSourcePool dell’AEM per gestire le connessioni.

## Supporto di rete avanzato

Il codice di esempio seguente è supportato dalle seguenti opzioni di rete avanzate.

Assicurati che [appropriato](../advanced-networking.md#advanced-networking) la configurazione di rete avanzata è stata impostata prima di seguire questa esercitazione.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Virtual Private Network](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configurazione OSGi

La stringa di connessione della configurazione OSGi utilizza:

+ `AEM_PROXY_HOST` tramite il [Variabile di ambiente di configurazione OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` come host della connessione
+ `30001` che è il `portOrig` valore per la mappatura in avanti della porta di Cloud Manager `30001` → `mysql.example.com:3306`

Poiché i segreti non devono essere archiviati nel codice, è consigliabile fornire il nome utente e la password della connessione SQL tramite le variabili di configurazione OSGi, impostate utilizzando la CLI AIO o le API di Cloud Manager.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

I seguenti elementi `aio CLI` Questo comando può essere utilizzato per impostare i segreti OSGi in base all’ambiente:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Esempio di codice

Questo esempio di codice Java™ è di un servizio OSGi che effettua una connessione a un database MySQL esterno tramite il servizio OSGi DataSourcePool dell’AEM.
La configurazione di fabbrica di DataSourcePool OSGi specifica una porta (`30001`) mappato tramite il `portForwards` regola in [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) funzionamento verso l&#39;host e la porta esterni, `mysql.example.com:3306`.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
    }
}
```

## Dipendenze driver MySQL

AEM as a Cloud Service richiede spesso di fornire driver di database Java™ per supportare le connessioni. Il modo migliore per fornire i driver consiste solitamente nell’incorporare nel progetto AEM gli artefatti del bundle OSGi contenenti tali driver, mediante `all` pacchetto.

### Reactor pom.xml

Includere le dipendenze dei driver di database nel reattore `pom.xml` e poi farvi riferimento in `all` sottoprogetti.

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## Tutti i file pom.xml

Incorporare gli artefatti di dipendenza del driver di database in `all` sono implementati e disponibili su AEM as a Cloud Service. Questi artefatti __deve__ essere bundle OSGi che esportano la classe Java™ del driver del database.

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
