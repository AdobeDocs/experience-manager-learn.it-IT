---
title: Uscita porta flessibile
description: Scopri come impostare e utilizzare l’uscita flessibile della porta per supportare connessioni esterne da AEM as a Cloud Service a servizi esterni.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
duration: 906
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 1%

---

# Uscita porta flessibile

Scopri come impostare e utilizzare l’uscita flessibile della porta per supportare connessioni esterne da AEM as a Cloud Service a servizi esterni.

## Cos’è l’uscita con porta flessibile?

L’uscita flessibile della porta consente di collegare a AEM as a Cloud Service regole specifiche e personalizzate per l’inoltro delle porte, consentendo di effettuare connessioni dall’AEM a servizi esterni.

Un programma Cloud Manager può avere solo __singolo__ tipo di infrastruttura di rete. Assicurati che l’indirizzo IP in uscita dedicato sia il più [tipo appropriato di infrastruttura di rete](./advanced-networking.md)  per l’AEM as a Cloud Service prima di eseguire i seguenti comandi.

>[!MORELIKETHIS]
>
> Leggere l’as a Cloud Service AEM [documentazione sulla configurazione di rete avanzata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) per ulteriori dettagli sull’uscita da porta flessibile.

## Prerequisiti

Quando si imposta l&#39;uscita di porta flessibile, è necessario quanto segue:

+ Progetto della console Adobe Developer con API di Cloud Manager abilitata e [Autorizzazioni di Proprietario business di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Accesso a [Credenziali di autenticazione dell’API di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID organizzazione (ID organizzazione IMS)
   + ID client (alias chiave API)
   + Token di accesso (token Bearer)
+ ID del programma Cloud Manager
+ ID dell’ambiente di Cloud Manager

Per ulteriori dettagli, consulta la procedura dettagliata seguente per scoprire come impostare, configurare e ottenere le credenziali API di Cloud Manager e come utilizzarle per effettuare una chiamata API di Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Questa esercitazione utilizza `curl` per effettuare le configurazioni API di Cloud Manager. Il fornito `curl` I comandi presuppongono una sintassi Linux/macOS. Se si utilizza il prompt dei comandi di Windows, sostituire `\` carattere di interruzione di riga con `^`.

## Abilita uscita porta flessibile per programma

Iniziare abilitando l&#39;uscita della porta flessibile su AEM as a Cloud Service.

1. Innanzitutto, determina l’area in cui è configurato il networking avanzato utilizzando l’API di Cloud Manager. [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Il `region name` è richiesto per effettuare chiamate API successive di Cloud Manager. In genere, viene utilizzata l’area in cui risiede l’ambiente di produzione.

   Trova l’area geografica dell’ambiente AEM as a Cloud Service in [Cloud Manager](https://my.cloudmanager.adobe.com) sotto [dettagli dell’ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Il nome dell’area visualizzato in Cloud Manager può essere [mappato al codice di regione](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments.it) utilizzato nell’API di Cloud Manager.

   __richiesta HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Abilitare l’uscita con porta flessibile per un programma Cloud Manager utilizzando l’API di Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Utilizza il `region` codice ottenuto dall’API di Cloud Manager `listRegions` operazione.

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

1. Verifica che l’ambiente sia stato completato __uscita porta flessibile__ configurazione tramite l’API di Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) funzionamento, utilizzando `id` restituito dalla richiesta HTTP createNetworkInfrastructure nel passaggio precedente.

   __richiesta HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifica che la risposta HTTP contenga un __stato__ di __pronto__. Se non è ancora pronto, ricontrolla lo stato ogni pochi minuti.

## Configurare proxy di uscita di porta flessibili per ambiente

1. Abilita e configura il __uscita porta flessibile__ configurazione in ogni ambiente AEM as a Cloud Service tramite l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione.

   __enableEnvironmentAdvancedNetworkingConfiguration richiesta HTTP__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Definire i parametri JSON in una `flexible-port-egress.json` e forniti a curl tramite `... -d @./flexible-port-egress.json`.

   [Scarica l’esempio Flexible-Port-egress.json](./assets/flexible-port-egress.json). Questo file è solo un esempio. Configura il file come richiesto in base ai campi facoltativi/obbligatori documentati in [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   Se l’implementazione dell’AEM __solo__ richiede connessioni HTTP/HTTPS (porta 80/443) al servizio esterno, lasciare `portForwards` array vuoto, in quanto queste regole sono necessarie solo per le richieste non HTTP/HTTPS.

1. Per ogni ambiente, verifica che le regole di uscita siano attive utilizzando l’API di Cloud Manager. [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione.

   __richiesta HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Le configurazioni di uscita della porta flessibile possono essere aggiornate utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Ricorda `enableEnvironmentAdvancedNetworkingConfiguration` è un `PUT` , pertanto tutte le regole devono essere fornite con ogni chiamata di questa operazione.

1. Ora puoi utilizzare la configurazione dell’uscita della porta flessibile nel codice AEM e nella configurazione personalizzati.


## Connessione a servizi esterni tramite uscita porta flessibile

Con il proxy di uscita con porta flessibile abilitato, il codice e la configurazione AEM possono utilizzarli per effettuare chiamate a servizi esterni. Esistono due tipi di chiamate esterne che l’AEM tratta in modo diverso:

1. Chiamate HTTP/HTTPS a servizi esterni su porte non standard
   + Include le chiamate HTTP/HTTPS effettuate a servizi in esecuzione su porte diverse dalle porte standard 80 o 443.
1. chiamate non HTTP/HTTPS a servizi esterni
   + Include tutte le chiamate non HTTP, ad esempio le connessioni con i server di posta, i database SQL o i servizi eseguiti su altri protocolli non HTTP/HTTPS.

Le richieste HTTP/HTTPS da AEM sulle porte standard (80/443) sono consentite per impostazione predefinita e non richiedono configurazioni o considerazioni aggiuntive.


### HTTP/HTTPS su porte non standard

Quando si creano connessioni HTTP/HTTPS a porte non standard (non-80/443) dall’AEM, le connessioni devono essere effettuate tramite host e porte speciali, forniti tramite segnaposto.

L’AEM fornisce due set di variabili speciali di sistema Java™ mappate ai proxy HTTP/HTTPS dell’AEM.

| Nome variabile | Utilizzare | Codice Java™ | Configurazione OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy per entrambe le connessioni HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | Porta proxy per connessioni HTTPS (imposta il fallback su `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | Porta proxy per connessioni HTTPS (imposta il fallback su `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Quando si effettuano chiamate HTTP/HTTPS a servizi esterni su porte non standard, non viene restituito alcun `portForwards` deve essere definito utilizzando l’API di Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` operazione, in quanto le &quot;regole&quot; di port forwarding sono definite &quot;nel codice&quot;.

>[!TIP]
>
> Consulta la documentazione sull’uscita della porta flessibile di AEM as a Cloud Service per [insieme completo di regole di instradamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### Esempi di codice

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="HTTP/HTTPS su porte non standard" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS su porte non standard</a></strong></div>
    <p>
        Esempio di codice Java™ per rendere la connessione HTTP/HTTPS da AEM as a Cloud Service a un servizio esterno su porte HTTP/HTTPS non standard.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Connessioni non HTTP/HTTPS a servizi esterni

Durante la creazione di connessioni non HTTP/HTTPS (ad es. SQL, SMTP e così via) dall’AEM, la connessione deve essere effettuata attraverso uno speciale nome host fornito dall’AEM.

| Nome variabile | Utilizzare | Codice Java™ | Configurazione OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy per connessioni non HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


Le connessioni ai servizi esterni vengono quindi richiamate tramite `AEM_PROXY_HOST` e la porta mappata (`portForwards.portOrig`), che AEM indirizza quindi al nome host esterno mappato (`portForwards.name`) e porta (`portForwards.portDest`).

| Host proxy | Porta proxy |  | Host esterno | Porta esterna |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Esempi di codice

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connessione SQL tramite DataSourcePool JDBC" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connessione SQL tramite DataSourcePool JDBC</a></strong></div>
      <p>
            Esempio di codice Java™ connessione a database SQL esterni tramite la configurazione del pool di origini dati JDBC dell'AEM.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Connessione SQL tramite API Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Connessione SQL tramite API Java™</a></strong></div>
      <p>
            Esempio di codice Java™ per la connessione a database SQL esterni tramite le API SQL di Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servizio di posta elettronica</a></strong></div>
      <p>
        Esempio di configurazione OSGi con AEM per la connessione a servizi di posta elettronica esterni.
      </p>
    </td>   
</tr></table>
