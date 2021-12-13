---
title: Connessioni SQL tramite API Java™
description: Scopri come connettersi ai database SQL da AEM as a Cloud Service tramite API Java™ SQL e porte di uscita.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9356
thumbnail: KT-9356.jpeg
exl-id: ec9d37cb-70b6-4414-a92b-3b84b3f458ab
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 0%

---

# Connessioni SQL tramite API Java™

Le connessioni ai database SQL (e ad altri servizi non HTTP/HTTPS) devono essere proxy fuori AEM.

L&#39;eccezione a questa regola è quando [indirizzo ip dedicato in uscita](../dedicated-egress-ip-address.md) è in uso e il servizio è in Adobe o Azure.

## Supporto di rete avanzato

L&#39;esempio di codice seguente è supportato dalle seguenti opzioni di rete avanzate.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Rete privata virtuale](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ↓ | ↓ | ↓ |

## Configurazione OSGi

Poiché i segreti non devono essere memorizzati nel codice, è consigliabile fornire il nome utente e la password della connessione SQL tramite [variabili di configurazione OSGi segrete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), impostato utilizzando AIO CLI o le API di Cloud Manager.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

I seguenti `aio CLI` può essere utilizzato per impostare i segreti OSGi in base all&#39;ambiente:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Esempio di codice

Questo esempio di codice Java™ è di un servizio OSGi che effettua una connessione a un server Web SQL server esterno, tramite il seguente Cloud Manager `portForwards` regola [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) funzionamento.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/MySqlExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.sql.*;

@Component
@Designate(ocd = MySqlExternalServiceImpl.Config.class)
public class MySqlExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(MySqlExternalServiceImpl.class);

    // Get the proxy host using the AEM_PROXY_HOST Java environment variable provided by AEM as a Cloud Service
    private static final String PROXY_HOST = System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel");

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = 30001;

    private Config config;

    @Override
    public boolean isAccessible() {
        log.debug("MySQL connection URL: [ {} ]", getConnectionUrl(config));

        // Establish a connection with the external MySQL service
        // This MySQL connection URL is created in getConnection(..) which will use the AEM_PROXY_HOST is it exists, and the proxied port.
        try (Connection connection = DriverManager.getConnection(getConnectionUrl(config), config.username(), config.password())) {
            // Validate the connection
            connection.isValid(1000);

            // Close the connection, since this is just a simple connectivity check
            connection.close();

            // Return true if AEM could reach the external SQL service
            return true;
        } catch (SQLException e) {
            log.error("Unable to establish an connection with MySQL service using connection URL  [ {} ]", getConnectionUrl(config), e);
        }

        return false;
    }

    /**
     * Create a connection string to the MySQL using the AEM-provided AEM_PROXY_HOST and portForwards.portOrg port
     * defined in the Cloud Manager API mapping.
     *
     * @param config OSGi configuration object
     * @return the MySQL connection URI
     */
    private String getConnectionUrl(Config config) {
        return String.format("jdbc:mysql://%s:%d/wknd-examples", PROXY_HOST, PORT_FORWARDS_PORT_ORIG);
    }

    @Activate
    protected void activate(Config config) throws ClassNotFoundException, SQLException {
        this.config = config;

        // Load the required MySQL Driver class required for Java to make the connection
        // The OSGi bundle that contains this driver is deployed via the project's all project
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
    }

    @ObjectClassDefinition
    @interface Config {
        @AttributeDefinition(type = AttributeType.STRING)
        String username();

        @AttributeDefinition(type = AttributeType.STRING)
        String password();
    }
}
```

## Dipendenze dei driver MySQL

AEM as a Cloud Service spesso richiede di fornire driver di database Java™ per supportare le connessioni. Fornire i driver è generalmente meglio ottenuto incorporando gli artefatti del bundle OSGi contenenti questi driver al progetto AEM tramite il `all` pacchetto.

### Reattore pom.xml

Includere le dipendenze del driver di database nel reattore `pom.xml` e poi fai riferimento a loro nel `all` sottoprogetti.

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

## Tutto pom.xml

Incorporare gli artefatti di dipendenza del driver di database in `all` Il pacchetto viene distribuito e disponibile su AEM as a Cloud Service. Questi artefatti __deve__ essere bundle OSGi che esportano la classe Java™ del driver di database.

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
