---
title: Virtual Private Network (VPN)
description: Scopri come collegare AEM as a Cloud Service con la tua VPN per creare canali di comunicazione sicuri tra AEM e servizi interni.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
duration: 948
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 2%

---

# Virtual Private Network (VPN)

Scopri come collegare AEM as a Cloud Service con la tua VPN per creare canali di comunicazione sicuri tra AEM e servizi interni.

## Che cos&#39;è Virtual Private Network?

Virtual Private Network (VPN) consente a un cliente as a Cloud Service AEM di connettersi **gli ambienti AEM** all’interno di un programma Cloud Manager a un [supportato](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) VPN. Ciò consente connessioni sicure e controllate tra l’AEM as a Cloud Service e i servizi all’interno della rete del cliente.

Un programma Cloud Manager può avere solo __singolo__ tipo di infrastruttura di rete. Garantire che la rete privata virtuale sia la più [tipo appropriato di infrastruttura di rete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#general-vpn-considerations) per l’AEM as a Cloud Service prima di eseguire i seguenti comandi.

>[!NOTE]
>
>Nota: la connessione dell’ambiente di build da Cloud Manager a una VPN non è supportata. Se devi accedere agli artefatti binari da un archivio privato, devi impostare un archivio protetto da password e protetto da password con un URL disponibile su Internet pubblica [come descritto qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> Leggere l’as a Cloud Service AEM [documentazione sulla configurazione di rete avanzata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) per ulteriori dettagli su Virtual Private Network.

## Prerequisiti

Durante la configurazione di una rete privata virtuale sono necessari i seguenti elementi:

+ Adobe account con [Autorizzazioni di Proprietario business di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Accesso a [Credenziali di autenticazione dell’API di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + ID organizzazione (ID organizzazione IMS)
   + ID client (alias chiave API)
   + Token di accesso (token Bearer)
+ ID del programma Cloud Manager
+ ID dell’ambiente di Cloud Manager
+ A **Basato su percorso** Virtual Private Network, con accesso a tutti i parametri di connessione necessari.

Per ulteriori dettagli, consulta la procedura dettagliata seguente per scoprire come impostare, configurare e ottenere le credenziali API di Cloud Manager e come utilizzarle per effettuare una chiamata API di Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Questa esercitazione utilizza `curl` per effettuare le configurazioni API di Cloud Manager. Il fornito `curl` I comandi presuppongono una sintassi Linux/macOS. Se si utilizza il prompt dei comandi di Windows, sostituire `\` carattere di interruzione di riga con `^`.

## Abilita rete privata virtuale per programma

Per iniziare, abilita la rete privata virtuale su AEM as a Cloud Service.

1. Innanzitutto, determina l’area in cui è necessaria la rete avanzata utilizzando l’API di Cloud Manager. [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Il `region name` è richiesto per effettuare chiamate API successive di Cloud Manager. In genere, viene utilizzata l’area in cui risiede l’ambiente di produzione.

   Trova l’area geografica dell’ambiente AEM as a Cloud Service in [Cloud Manager](https://my.cloudmanager.adobe.com) sotto [dettagli dell’ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Il nome dell’area visualizzato in Cloud Manager può essere [mappato al codice di regione](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments.it) utilizzato nell’API di Cloud Manager.

   __richiesta HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Abilitare Virtual Private Network per un programma Cloud Manager utilizzando le API di Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Utilizza il `region` codice ottenuto dall’API di Cloud Manager `listRegions` operazione.

   __richiesta HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Definire i parametri JSON in una `vpn-create.json` e forniti a curl tramite `... -d @./vpn-create.json`.

   [Scarica l’esempio vpn-create.json](./assets/vpn-create.json).  Questo file è solo un esempio. Configura il file come richiesto in base ai campi facoltativi/obbligatori documentati in [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Attendi 45-60 minuti affinché il programma Cloud Manager esegua il provisioning dell’infrastruttura di rete.

1. Verifica che l’ambiente sia stato completato __Virtual Private Network__ configurazione tramite l’API di Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) funzionamento, utilizzando `id` restituito dalla richiesta HTTP createNetworkInfrastructure nel passaggio precedente.

   __richiesta HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifica che la risposta HTTP contenga un __stato__ di __pronto__. Se non è ancora pronto, ricontrolla lo stato ogni pochi minuti.

## Configurare proxy di rete privata virtuale per ambiente

1. Abilita e configura il __Virtual Private Network__ configurazione in ogni ambiente AEM as a Cloud Service tramite l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione.

   __enableEnvironmentAdvancedNetworkingConfiguration richiesta HTTP__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Definire i parametri JSON in una `vpn-configure.json` e forniti a curl tramite `... -d @./vpn-configure.json`.

[Scarica l’esempio vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
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

   `nonProxyHosts` dichiara un set di host per i quali la porta 80 o 443 deve essere instradata attraverso gli intervalli di indirizzi IP condivisi predefiniti anziché attraverso l’IP in uscita dedicato. `nonProxyHosts` può essere utile in quanto, ad Adobe, il traffico in uscita attraverso gli IP condivisi può essere ulteriormente ottimizzato automaticamente.

   Per ogni `portForwards` mappatura, la rete avanzata definisce la seguente regola di inoltro:

   | Host proxy | Porta proxy |  | Host esterno | Porta esterna |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Se l’implementazione dell’AEM __solo__ richiede connessioni HTTP/HTTPS a un servizio esterno, lascia `portForwards` array vuoto, in quanto queste regole sono necessarie solo per le richieste non HTTP/HTTPS.


1. Per ogni ambiente, verifica che le regole di routing della VPN siano attive utilizzando l’API di Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione.

   __richiesta HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Le configurazioni proxy di rete privata virtuale possono essere aggiornate utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Ricorda `enableEnvironmentAdvancedNetworkingConfiguration` è un `PUT` , pertanto tutte le regole devono essere fornite con ogni chiamata di questa operazione.

1. Ora puoi utilizzare la configurazione di uscita Virtual Private Network nel codice AEM e nella configurazione personalizzati.

## Connessione a servizi esterni tramite la rete privata virtuale

Con la rete privata virtuale abilitata, il codice e la configurazione AEM possono utilizzarli per effettuare chiamate a servizi esterni tramite la VPN. Esistono due tipi di chiamate esterne che l’AEM tratta in modo diverso:

1. Chiamate HTTP/HTTPS a servizi esterni
   + Include le chiamate HTTP/HTTPS effettuate a servizi in esecuzione su porte diverse dalle porte standard 80 o 443.
1. chiamate non HTTP/HTTPS a servizi esterni
   + Include tutte le chiamate non HTTP, ad esempio le connessioni con i server di posta, i database SQL o i servizi eseguiti su altri protocolli non HTTP/HTTPS.

Le richieste HTTP/HTTPS dall’AEM sulle porte standard (80/443) sono consentite per impostazione predefinita, ma non utilizzeranno la connessione VPN se non configurate in modo appropriato come descritto di seguito.

### HTTP/HTTPS

Quando si creano connessioni HTTP/HTTPS dall’AEM, quando si utilizza la VPN le connessioni HTTP/HTTPS vengono automaticamente escluse dall’AEM. Per supportare le connessioni HTTP/HTTPS non è necessario alcun codice o configurazione aggiuntivi.

>[!TIP]
>
> Consulta la documentazione Virtual Private Network di AEM as a Cloud Service per [insieme completo di regole di instradamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### Esempi di codice

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Esempio di codice Java™ per rendere la connessione HTTP/HTTPS dall’AEM as a Cloud Service a un servizio esterno utilizzando il protocollo HTTP/HTTPS.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Esempi di codice di connessioni non HTTP/HTTPS

Durante la creazione di connessioni non HTTP/HTTPS (ad es. SQL, SMTP e così via) dall’AEM, la connessione deve essere effettuata attraverso uno speciale nome host fornito dall’AEM.

| Nome variabile | Utilizzare | Codice Java™ | Configurazione OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy per connessioni non HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Le connessioni ai servizi esterni vengono quindi richiamate tramite `AEM_PROXY_HOST` e la porta mappata (`portForwards.portOrig`), che AEM indirizza quindi al nome host esterno mappato (`portForwards.name`) e porta (`portForwards.portDest`).

| Host proxy | Porta proxy |  | Host esterno | Porta esterna |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### Esempi di codice

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connessione SQL tramite DataSourcePool JDBC" src="./assets//code-examples__sql-osgi.png"/></a>
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

### Limitare l’accesso all’AEM as a Cloud Service tramite VPN

La configurazione Virtual Private Network limita l&#39;accesso agli ambienti as a Cloud Service AEM a una VPN.

#### Esempi di configurazione

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Applicazione di un elenco consentiti IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Applicazione di un elenco Consentiti IP di</a></strong></div>
      <p>
            Configurare un elenco Consentiti di accesso IP in modo che solo il traffico VPN possa accedere all’AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Limitazioni dell’accesso VPN basato su percorsi per la pubblicazione AEM" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Limitazioni dell’accesso VPN basato su percorsi per la pubblicazione AEM</a></strong></div>
      <p>
            Richiedi accesso VPN per percorsi specifici nella pubblicazione AEM.
      </p>
    </td>
   <td></td>
</tr></table>
