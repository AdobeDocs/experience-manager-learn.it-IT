---
title: Virtual Private Network (VPN)
description: Scopri come collegare AEM as a Cloud Service con la tua VPN per creare canali di comunicazione sicuri tra AEM e servizi interni.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '1370'
ht-degree: 1%

---

# Virtual Private Network (VPN)

Scopri come collegare AEM as a Cloud Service con la tua VPN per creare canali di comunicazione sicuri tra AEM e servizi interni.

## Che cos’è Virtual Private Network?

Virtual Private Network (VPN) consente a un cliente as a Cloud Service AEM di connettersi **gli ambienti AEM** all’interno di un programma Cloud Manager per un [supportato](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) VPN. Ciò consente connessioni sicure e controllate tra AEM servizi as a Cloud Service e all&#39;interno della rete del cliente.

Un programma Cloud Manager può avere solo un __singolo__ tipo di infrastruttura di rete. Assicurati che la rete privata virtuale sia la migliore [tipo adeguato di infrastruttura di rete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#general-vpn-considerations) per la AEM as a Cloud Service prima di eseguire i seguenti comandi.

>[!NOTE]
>
>La connessione dell’ambiente di build da Cloud Manager a una VPN non è supportata. Se devi accedere agli artefatti binari da un archivio privato, devi configurare un archivio protetto e protetto da password con un URL disponibile su Internet pubblico [come descritto qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> Leggi la AEM as a Cloud Service [documentazione avanzata sulla configurazione della rete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) per ulteriori informazioni su Virtual Private Network.

## Prerequisiti

Quando si configura Virtual Private Network sono necessari i seguenti requisiti:

+ Adobe account con [Autorizzazioni di Business Owner di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Accesso a [Credenziali di autenticazione dell’API di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID organizzazione (alias ID organizzazione IMS)
   + ID client (alias chiave API)
   + Token di accesso (alias Token portatore)
+ ID del programma Cloud Manager
+ ID ambiente di Cloud Manager
+ Una rete privata virtuale con accesso a tutti i parametri di connessione necessari.

Per ulteriori dettagli, consulta la procedura dettagliata seguente per scoprire come configurare, configurare e ottenere le credenziali API di Cloud Manager e come utilizzarle per effettuare una chiamata API di Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

Questa esercitazione utilizza `curl` per configurare le configurazioni API di Cloud Manager. Il `curl` i comandi si basano su una sintassi Linux/macOS. Se utilizzi il prompt dei comandi di Windows, sostituisci il `\` carattere di interruzione di riga con `^`.

## Abilita rete privata virtuale per programma

Inizia abilitando Virtual Private Network su AEM as a Cloud Service.

1. Innanzitutto, determina la regione in cui verrà impostata la rete avanzata utilizzando l’API di Cloud Manager. [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) funzionamento. La `region name` sarà richiesto per effettuare chiamate API di Cloud Manager successive. In genere, viene utilizzata l&#39;area in cui si trova l&#39;ambiente Produzione.

   __richiesta HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Abilitare Virtual Private Network per un programma Cloud Manager utilizzando le API di Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) funzionamento. Utilizza le `region` codice ottenuto dall’API di Cloud Manager `listRegions` funzionamento.

   __richiesta HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Definire i parametri JSON in un `vpn-create.json` e fornito per curl tramite `... -d @./vpn-create.json`.

   [Scarica l&#39;esempio vpn-create.json](./assets/vpn-create.json).  Questo file è solo un esempio. Configura il file come richiesto in base ai campi facoltativi/obbligatori documentati in [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

1. Verifica che l’ambiente sia stato completato __Rete privata virtuale__ configurazione tramite l’API di Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) utilizzando `id` restituito dalla richiesta HTTP createNetworkInfrastructure nel passaggio precedente.

   __richiesta HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifica che la risposta HTTP contenga un __status__ di __ready__. Se non è ancora pronto, controlla di nuovo lo stato ogni pochi minuti.

## Configurare proxy di rete privata virtuale per ambiente

1. Abilita e configura le __Rete privata virtuale__ configurazione su ogni ambiente as a Cloud Service AEM utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) funzionamento.

   __richiesta HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Definire i parametri JSON in un `vpn-configure.json` e fornito per curl tramite `... -d @./vpn-configure.json`.

[Scarica l&#39;esempio vpn-configure.json](./assets/vpn-configure.json)

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

   `nonProxyHosts` dichiara un set di host per i quali la porta 80 o 443 deve essere instradata attraverso gli intervalli di indirizzi IP condivisi predefiniti anziché l&#39;IP di uscita dedicato. `nonProxyHosts` può essere utile in quanto il traffico che attraversa IP condivisi può essere ulteriormente ottimizzato automaticamente tramite Adobe.

   Per ogni `portForwards` mappatura, la rete avanzata definisce la seguente regola di inoltro:

   | Host proxy | Porta proxy |  | Host esterno | Porta esterna |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Se la distribuzione AEM __only__ richiede connessioni HTTP/HTTPS a un servizio esterno, lascia `portForwards` array vuoto, in quanto queste regole sono necessarie solo per le richieste non HTTP/HTTPS.


1. Per ogni ambiente, convalida le regole di indirizzamento vpn in vigore utilizzando l’API di Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) funzionamento.

   __richiesta HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Le configurazioni proxy di rete privata virtuale possono essere aggiornate utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) funzionamento. Ricorda `enableEnvironmentAdvancedNetworkingConfiguration` è un `PUT` quindi tutte le regole devono essere fornite con ogni chiamata di questa operazione.

1. Ora puoi utilizzare la configurazione dell’uscita Virtual Private Network nel codice AEM e nella configurazione personalizzati.

## Connessione a servizi esterni tramite Virtual Private Network

Con Virtual Private Network abilitato, il codice AEM e la configurazione possono utilizzarli per effettuare chiamate a servizi esterni tramite VPN. Esistono due tipi di chiamate esterne che AEM trattate in modo diverso:

1. Chiamate HTTP/HTTPS a servizi esterni
   + Include le chiamate HTTP/HTTPS effettuate a servizi in esecuzione su porte diverse dalle porte standard 80 o 443.
1. chiamate non HTTP/HTTPS a servizi esterni
   + Include tutte le chiamate non HTTP, come le connessioni con server di posta, database SQL o servizi che vengono eseguite su altri protocolli non HTTP/HTTPS.

Le richieste HTTP/HTTPS da AEM sulle porte standard (80/443) sono consentite per impostazione predefinita, ma non utilizzeranno la connessione VPN se non è configurata correttamente come descritto di seguito.

### HTTP/HTTPS

Durante la creazione di connessioni HTTP/HTTPS da AEM, per ottenere un indirizzo IP in uscita dedicato o essere indirizzato tramite VPN, la connessione deve essere effettuata tramite host e porte speciali, forniti tramite segnaposto.

AEM fornisce due set di variabili di sistema Java™ speciali associate a proxy HTTP/HTTPS AEM.

| Nome della variabile | Uso | Codice Java™ | Configurazione OSGi | Configurazione mod_proxy del server web Apache | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Host proxy per connessioni HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | Porta proxy per connessioni HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | Host proxy per connessioni HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | Porta proxy per connessioni HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

Le richieste ai servizi esterni HTTP/HTTPS devono essere effettuate configurando la configurazione proxy del client Java™ HTTP tramite AEM valori host/porte proxy.

Quando si eseguono chiamate HTTP/HTTPS a servizi esterni su qualsiasi porta, non corrisponde `portForwards` deve essere definito utilizzando le API di Cloud Manager `__enableEnvironmentAdvancedNetworkingConfiguration` , in quanto le &quot;regole&quot; di inoltro della porta sono definite &quot;nel codice&quot;.

>[!TIP]
>
> Consulta AEM documentazione di Virtual Private Network di as a Cloud Service per [l&#39;intero insieme di regole di routing](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### Esempi di codice

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Esempio di codice Java™ per la connessione HTTP/HTTPS da AEM as a Cloud Service a un servizio esterno tramite il protocollo HTTP/HTTPS.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Esempi di codice per le connessioni non HTTP/HTTPS

Durante la creazione di connessioni non HTTP/HTTPS (ad esempio. SQL, SMTP e così via) da AEM, la connessione deve essere effettuata attraverso un nome host speciale fornito da AEM.

| Nome della variabile | Uso | Codice Java™ | Configurazione OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Host proxy per connessioni non HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Le connessioni a servizi esterni vengono quindi chiamate attraverso `AEM_PROXY_HOST` e la porta mappata (`portForwards.portOrig`), che AEM quindi effettua l’indirizzamento al nome host esterno mappato (`portForwards.name`) e la porta (`portForwards.portDest`).

| Host proxy | Porta proxy |  | Host esterno | Porta esterna |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### Esempi di codice

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connessione SQL tramite JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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

### Limitare l’accesso a AEM as a Cloud Service tramite VPN

La configurazione Virtual Private Network limita l’accesso AEM ambienti as a Cloud Service a una VPN.

#### Esempi di configurazione

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Applicazione di un elenco consentiti IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Applicazione di un inserire nell'elenco Consentiti IP</a></strong></div>
      <p>
            Configura un inserire nell'elenco Consentiti IP in modo che solo il traffico VPN possa accedere a AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Restrizioni di accesso VPN basate sul percorso per AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Restrizioni di accesso VPN basate sul percorso per AEM Publish</a></strong></div>
      <p>
            Richiedi l’accesso VPN per percorsi specifici su AEM Publish.
      </p>
    </td>
   <td></td>
</tr></table>
