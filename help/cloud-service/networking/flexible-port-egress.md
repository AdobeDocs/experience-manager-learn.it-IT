---
title: Uscita porta flessibile
description: Scopri come impostare e utilizzare un'uscita di porta flessibile per supportare connessioni esterne da AEM as a Cloud Service a servizi esterni.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '1035'
ht-degree: 0%

---

# Uscita porta flessibile

Scopri come impostare e utilizzare un&#39;uscita di porta flessibile per supportare connessioni esterne da AEM as a Cloud Service a servizi esterni.

## Che cos&#39;è l&#39;uscita della porta flessibile?

L&#39;uscita di porta flessibile consente di allegare a AEM as a Cloud Service regole di inoltro di porta specifiche e personalizzate, consentendo connessioni da AEM a servizi esterni.

Un programma Cloud Manager può avere solo un __singolo__ tipo di infrastruttura di rete. Assicurati che l&#39;indirizzo IP dedicato in uscita sia il più importante [tipo adeguato di infrastruttura di rete](./advanced-networking.md)  per la AEM as a Cloud Service prima di eseguire i seguenti comandi.

>[!MORELIKETHIS]
>
> Leggi la AEM as a Cloud Service [documentazione avanzata sulla configurazione della rete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#flexible-port-egress) per ulteriori dettagli sull&#39;uscita della porta flessibile.

## Prerequisiti

Quando si imposta un&#39;uscita di porta flessibile sono necessari i seguenti requisiti:

+ Progetto di Adobe I/O con l’API di Cloud Manager abilitata e [Autorizzazioni di Business Owner di Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Accesso a [Credenziali di autenticazione dell’API di Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID organizzazione (alias ID organizzazione IMS)
   + ID client (alias chiave API)
   + Token di accesso (alias Token portatore)
+ ID del programma Cloud Manager
+ ID ambiente di Cloud Manager

Questa esercitazione utilizza `curl` per configurare le configurazioni API di Cloud Manager. Il `curl` i comandi si basano su una sintassi Linux/macOS. Se utilizzi il prompt dei comandi di Windows, sostituisci il `\` carattere di interruzione di riga con `^`.

## Abilita uscita porta flessibile per programma

Inizia abilitando l&#39;uscita della porta flessibile su AEM as a Cloud Service.

1. Innanzitutto, determina la regione in cui verrà impostata la rete avanzata utilizzando l’API di Cloud Manager. [listRegion](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) funzionamento. La `region name` sarà richiesto per effettuare chiamate API di Cloud Manager successive. In genere, viene utilizzata l&#39;area in cui si trova l&#39;ambiente Produzione.

   __richiesta HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Abilitare l’uscita della porta flessibile per un programma Cloud Manager tramite l’API di Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) funzionamento. Utilizza le `region` codice ottenuto dall’API di Cloud Manager `listRegions` funzionamento.

   __richiesta HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Attendi 15 minuti affinché il programma Cloud Manager esegua il provisioning dell’infrastruttura di rete.

1. Verifica che l’ambiente sia stato completato __uscita porta flessibile__ configurazione tramite l’API di Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) utilizzando `id` restituito dalla richiesta HTTP createNetworkInfrastructure nel passaggio precedente.

   __richiesta HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifica che la risposta HTTP contenga un __status__ di __ready__. Se non è ancora pronto, controlla di nuovo lo stato ogni pochi minuti.

## Configura proxy di uscita flessibili per ogni ambiente

1. Abilita e configura le __uscita porta flessibile__ configurazione su ogni ambiente as a Cloud Service AEM utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) funzionamento.

   __richiesta HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Definire i parametri JSON in un `flexible-port-egress.json` e fornito per curl tramite `... -d @./flexible-port-egress.json`.

[Scarica l&#39;esempio Flexible-port-egress.json](./assets/flexible-port-egress.json)

   ```json
   {
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   Per ogni `portForwards` mappatura, la rete avanzata definisce la seguente regola di inoltro:

   | Host proxy | Porta proxy |  | Host esterno | Porta esterna |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Se la distribuzione AEM __only__ richiede connessioni HTTP/HTTPS (porta 80/443) al servizio esterno, lascia `portForwards` array vuoto, in quanto queste regole sono necessarie solo per le richieste non HTTP/HTTPS.

1. Per ogni ambiente, convalida le regole di uscita effettive utilizzando l’API di Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) funzionamento.

   __richiesta HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Le configurazioni di uscita delle porte flessibili possono essere aggiornate utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) funzionamento. Ricorda `enableEnvironmentAdvancedNetworkingConfiguration` è un `PUT` quindi tutte le regole devono essere fornite con ogni chiamata di questa operazione.

1. Ora puoi utilizzare la configurazione flessibile dell’uscita della porta nel codice AEM e nella configurazione personalizzati.


## Collegamento a servizi esterni tramite uscita di porta flessibile

Con il proxy di uscita della porta flessibile abilitato, il codice AEM e la configurazione possono utilizzarli per effettuare chiamate a servizi esterni. Esistono due tipi di chiamate esterne che AEM trattate in modo diverso:

1. Chiamate HTTP/HTTPS a servizi esterni su porte non standard
   + Include le chiamate HTTP/HTTPS effettuate a servizi in esecuzione su porte diverse dalle porte standard 80 o 443.
1. chiamate non HTTP/HTTPS a servizi esterni
   + Include tutte le chiamate non HTTP, come le connessioni con server di posta, database SQL o servizi che vengono eseguite su altri protocolli non HTTP/HTTPS.

Le richieste HTTP/HTTPS da AEM sulle porte standard (80/443) sono consentite per impostazione predefinita e non richiedono configurazioni o considerazioni aggiuntive.


### HTTP/HTTPS su porte non standard

Quando si creano connessioni HTTP/HTTPS a porte non standard (not-80/443) da AEM, le connessioni devono essere effettuate tramite host e porte speciali, forniti tramite segnaposto.

AEM fornisce due set di variabili di sistema Java™ speciali associate a proxy HTTP/HTTPS AEM.

| Nome della variabile | Uso | Codice Java™ | Configurazione OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Host proxy per entrambe le connessioni HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` | | `AEM_HTTP_PROXY_PORT` | Porta proxy per le connessioni HTTPS (impostare il fallback su `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` | | `AEM_HTTPS_PROXY_PORT` | Porta proxy per le connessioni HTTPS (impostare il fallback su `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Quando si eseguono chiamate HTTP/HTTPS a servizi esterni su porte non standard, non corrisponde `portForwards` deve essere definito utilizzando l’API di Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` , in quanto le &quot;regole&quot; di inoltro della porta sono definite &quot;nel codice&quot;.

>[!TIP]
>
> Consulta la documentazione sull&#39;uscita della porta flessibile di AEM as a Cloud Service per [l&#39;intero insieme di regole di routing](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### Esempi di codice

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS su porte non standard" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS su porte non standard</a></strong></div>
    <p>
        Esempio di codice Java™ per la connessione HTTP/HTTPS da AEM as a Cloud Service a un servizio esterno su porte HTTP/HTTPS non standard.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Connessioni non HTTP/HTTPS a servizi esterni

Durante la creazione di connessioni non HTTP/HTTPS (ad esempio. SQL, SMTP e così via) da AEM, la connessione deve essere effettuata attraverso un nome host speciale fornito da AEM.

| Nome della variabile | Uso | Codice Java™ | Configurazione OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Host proxy per connessioni non HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


Le connessioni a servizi esterni vengono quindi chiamate attraverso `AEM_PROXY_HOST` e la porta mappata (`portForwards.portOrig`), che AEM quindi effettua l’indirizzamento al nome host esterno mappato (`portForwards.name`) e la porta (`portForwards.portDest`).

| Host proxy | Porta proxy |  | Host esterno | Porta esterna |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Esempi di codice

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connessione SQL tramite JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connessione SQL tramite JDBC DataSourcePool</a></strong></div>
      <p>
            Esempio di codice Java™ che si collega a database SQL esterni configurando AEM pool di origini dati JDBC.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Connessione SQL tramite API Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Connessione SQL tramite API Java™</a></strong></div>
      <p>
            Esempio di codice Java™ per la connessione a database SQL esterni utilizzando le API SQL di Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servizio e-mail</a></strong></div>
      <p>
        Esempio di configurazione OSGi tramite AEM per la connessione a servizi e-mail esterni.
      </p>
    </td>   
</tr></table>
