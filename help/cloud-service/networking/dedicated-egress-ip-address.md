---
title: Indirizzo IP in uscita dedicato
description: Scopri come impostare e utilizzare l’indirizzo IP in uscita dedicato, che consente alle connessioni in uscita dall’AEM di provenire da un IP dedicato.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
duration: 926
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 1%

---

# Indirizzo IP in uscita dedicato

Scopri come impostare e utilizzare l’indirizzo IP in uscita dedicato, che consente alle connessioni in uscita dall’AEM di provenire da un IP dedicato.

## Cos’è l’indirizzo IP in uscita dedicato?

L’indirizzo IP in uscita dedicato consente alle richieste dell’AEM as a Cloud Service di utilizzare un indirizzo IP dedicato, consentendo ai servizi esterni di filtrare le richieste in ingresso in base a tale indirizzo IP. Mi piace [porte di uscita flessibili](./flexible-port-egress.md), l’IP in uscita dedicato consente l’uscita su porte non standard.

Un programma Cloud Manager può avere solo __singolo__ tipo di infrastruttura di rete. Assicurati che l’indirizzo IP in uscita dedicato sia il più [tipo appropriato di infrastruttura di rete](./advanced-networking.md)  per l’AEM as a Cloud Service prima di eseguire i seguenti comandi.

>[!MORELIKETHIS]
>
> Leggere l’as a Cloud Service AEM [documentazione sulla configurazione di rete avanzata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) per ulteriori dettagli sull’indirizzo IP in uscita dedicato.

## Prerequisiti

Durante la configurazione dell’indirizzo IP in uscita dedicato sono necessari i seguenti elementi:

+ API di Cloud Manager con [Autorizzazioni di Proprietario business di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Accesso a [Credenziali di autenticazione API di Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID organizzazione (ID organizzazione IMS)
   + ID client (alias chiave API)
   + Token di accesso (token Bearer)
+ ID del programma Cloud Manager
+ ID dell’ambiente di Cloud Manager

Per ulteriori dettagli, consulta la procedura dettagliata seguente per scoprire come impostare, configurare e ottenere le credenziali API di Cloud Manager e come utilizzarle per effettuare una chiamata API di Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Questa esercitazione utilizza `curl` per effettuare le configurazioni API di Cloud Manager. Il fornito `curl` I comandi presuppongono una sintassi Linux/macOS. Se si utilizza il prompt dei comandi di Windows, sostituire `\` carattere di interruzione di riga con `^`.

## Abilita indirizzo IP in uscita dedicato sul programma

Per iniziare, abilita e configura l’indirizzo IP in uscita dedicato su AEM as a Cloud Service.

1. Innanzitutto, determina l’area geografica in cui è necessaria la rete avanzata, utilizzando l’API di Cloud Manager. [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Il `region name` è richiesto per effettuare chiamate API successive di Cloud Manager. In genere, viene utilizzata l’area in cui risiede l’ambiente di produzione.

   Trova l’area geografica dell’ambiente AEM as a Cloud Service in [Cloud Manager](https://my.cloudmanager.adobe.com) sotto [dettagli dell’ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Il nome dell’area visualizzato in Cloud Manager può essere [mappato al codice di regione](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments.it) utilizzato nell’API di Cloud Manager.

   __richiesta HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Abilitare l’indirizzo IP in uscita dedicato per un programma Cloud Manager utilizzando l’API di Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Utilizza il `region` codice ottenuto dall’API di Cloud Manager `listRegions` operazione.

   __richiesta HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Attendi 15 minuti affinché il programma Cloud Manager esegua il provisioning dell’infrastruttura di rete.

1. Verifica che il programma sia stato completato __indirizzo IP in uscita dedicato__ configurazione tramite l’API di Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) funzionamento, utilizzando `id` restituito dalla richiesta HTTP createNetworkInfrastructure nel passaggio precedente.

   __richiesta HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifica che la risposta HTTP contenga un __stato__ di __pronto__. Se non è ancora pronto, ricontrolla lo stato ogni pochi minuti.

## Configurare proxy di indirizzi IP in uscita dedicati per ambiente

1. Configurare __indirizzo IP in uscita dedicato__ configurazione in ogni ambiente AEM as a Cloud Service tramite l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione.

   __enableEnvironmentAdvancedNetworkingConfiguration richiesta HTTP__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Definire i parametri JSON in una `dedicated-egress-ip-address.json` e forniti a curl tramite `... -d @./dedicated-egress-ip-address.json`.

   [Scarica l’esempio di indirizzo IP in uscita dedicato.json](./assets/dedicated-egress-ip-address.json). Questo file è solo un esempio. Configura il file come richiesto in base ai campi facoltativi/obbligatori documentati in [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   La firma HTTP della configurazione dell’indirizzo IP in uscita dedicato è diversa solo da [porta di uscita flessibile](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) in quanto supporta anche il `nonProxyHosts` configurazione.

   `nonProxyHosts` dichiara un set di host per i quali la porta 80 o 443 deve essere instradata attraverso gli intervalli di indirizzi IP condivisi predefiniti anziché attraverso l’IP in uscita dedicato. `nonProxyHosts` può essere utile in quanto, ad Adobe, il traffico in uscita attraverso gli IP condivisi può essere ulteriormente ottimizzato automaticamente.

   Per ogni `portForwards` mappatura, la rete avanzata definisce la seguente regola di inoltro:

   | Host proxy | Porta proxy |  | Host esterno | Porta esterna |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Per ogni ambiente, verifica che le regole di uscita siano attive utilizzando l’API di Cloud Manager. [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione.

   __richiesta HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Le configurazioni degli indirizzi IP in uscita dedicati possono essere aggiornate utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operazione. Ricorda `enableEnvironmentAdvancedNetworkingConfiguration` è un `PUT` , pertanto tutte le regole devono essere fornite con ogni chiamata di questa operazione.

1. Ottenere il __indirizzo IP in uscita dedicato__ utilizzando un sistema di risoluzione DNS (ad esempio [DNSChecker.org](https://dnschecker.org/)) sull&#39;host: `p{programId}.external.adobeaemcloud.com`, o eseguendo `dig` dalla riga di comando.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   Il nome host non può essere `pinged`, in quanto è un’uscita e _non_ e l&#39;ingresso.

   Osserva __indirizzo IP in uscita dedicato__ è condiviso da tutti gli ambienti AEM as a Cloud Service nel programma.

1. Ora puoi utilizzare l’indirizzo IP in uscita dedicato nel codice AEM e nella configurazione personalizzati. Spesso quando si utilizza un indirizzo IP in uscita dedicato, i servizi esterni a cui l’AEM as a Cloud Service si connette sono configurati per consentire solo il traffico da questo indirizzo IP dedicato.

## Connessione a servizi esterni tramite indirizzo IP in uscita dedicato

Con l’indirizzo IP in uscita dedicato abilitato, il codice e la configurazione AEM possono utilizzare l’IP in uscita dedicato per effettuare chiamate a servizi esterni. Esistono due tipi di chiamate esterne che l’AEM tratta in modo diverso:

1. Chiamate HTTP/HTTPS a servizi esterni
   + Include le chiamate HTTP/HTTPS effettuate a servizi in esecuzione su porte diverse dalle porte standard 80 o 443.
1. chiamate non HTTP/HTTPS a servizi esterni
   + Include tutte le chiamate non HTTP, ad esempio le connessioni con i server di posta, i database SQL o i servizi eseguiti su altri protocolli non HTTP/HTTPS.

Le richieste HTTP/HTTPS dall’AEM sulle porte standard (80/443) sono consentite per impostazione predefinita, ma non utilizzeranno l’indirizzo IP in uscita dedicato se non sono configurate in modo appropriato come descritto di seguito.

>[!TIP]
>
> Consulta la documentazione dedicata dell’indirizzo IP in uscita dell’as a Cloud Service AEM per [insieme completo di regole di instradamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

Quando si creano connessioni HTTP/HTTPS dall’AEM, quando si utilizza un indirizzo IP in uscita dedicato, le connessioni HTTP/HTTPS vengono automaticamente escluse dall’AEM utilizzando l’indirizzo IP in uscita dedicato. Per supportare le connessioni HTTP/HTTPS non è necessario alcun codice o configurazione aggiuntivi.

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

### Connessioni non HTTP/HTTPS a servizi esterni

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
