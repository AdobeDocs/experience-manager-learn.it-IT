---
title: Indirizzo IP in uscita dedicato
description: Scopri come impostare e utilizzare l’indirizzo IP in uscita dedicato, che consente alle connessioni in uscita da AEM di provenire da un IP dedicato.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '1186'
ht-degree: 0%

---


# Indirizzo IP in uscita dedicato

Scopri come impostare e utilizzare l’indirizzo IP in uscita dedicato, che consente alle connessioni in uscita da AEM di provenire da un IP dedicato.

## Qual è l’indirizzo IP in uscita dedicato?

L’indirizzo IP in uscita dedicato consente alle richieste di AEM as a Cloud Service di utilizzare un indirizzo IP dedicato, consentendo ai servizi esterni di filtrare le richieste in arrivo da questo indirizzo IP. Simile [porte di uscita flessibili](./flexible-port-egress.md), l’IP in uscita dedicato consente di uscire su porte non standard.

Un programma Cloud Manager può avere solo un __singolo__ tipo di infrastruttura di rete. Assicurati che l&#39;indirizzo IP dedicato in uscita sia il più importante [tipo adeguato di infrastruttura di rete](./advanced-networking.md)  per la AEM as a Cloud Service prima di eseguire i seguenti comandi.

>[!MORELIKETHIS]
>
> Leggi la AEM as a Cloud Service [documentazione avanzata sulla configurazione della rete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#dedicated-egress-IP-address) per maggiori dettagli sull&#39;indirizzo IP dedicato in uscita.

## Prerequisiti

Quando si imposta un indirizzo IP di uscita dedicato, sono necessari i seguenti requisiti:

+ API di Cloud Manager con [Autorizzazioni di Business Owner di Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Accesso a [Credenziali di autenticazione API di Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID organizzazione (alias ID organizzazione IMS)
   + ID client (alias chiave API)
   + Token di accesso (alias Token portatore)
+ ID del programma Cloud Manager
+ ID ambiente di Cloud Manager

Questa esercitazione utilizza `curl` per configurare le configurazioni API di Cloud Manager. Il `curl` i comandi si basano su una sintassi Linux/macOS. Se utilizzi il prompt dei comandi di Windows, sostituisci il `\` carattere di interruzione di riga con `^`.

## Attiva indirizzo IP in uscita dedicato sul programma

Inizia abilitando e configurando l’indirizzo IP di uscita dedicato su AEM as a Cloud Service.

1. Innanzitutto, determina la regione in cui verrà impostata la rete avanzata utilizzando l’API di Cloud Manager. [listRegion](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) funzionamento. La `region name` sarà richiesto per effettuare chiamate API di Cloud Manager successive. In genere, viene utilizzata l&#39;area in cui si trova l&#39;ambiente Produzione.

   __richiesta HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Abilitare l’indirizzo IP in uscita dedicato per un programma Cloud Manager tramite l’API Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) funzionamento. Utilizza le `region` codice ottenuto dall’API di Cloud Manager `listRegions` funzionamento.

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

1. Verifica che l’ambiente sia stato completato __indirizzo IP in uscita dedicato__ configurazione tramite l’API di Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) utilizzando `id` restituito dalla richiesta HTTP createNetworkInfrastructure nel passaggio precedente.

   __richiesta HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifica che la risposta HTTP contenga un __status__ di __ready__. Se non è ancora pronto, controlla di nuovo lo stato ogni pochi minuti.

## Configura proxy di indirizzi IP in uscita dedicati per ogni ambiente

1. Abilita e configura le __indirizzo IP in uscita dedicato__ configurazione su ogni ambiente as a Cloud Service AEM utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) funzionamento.

   __richiesta HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Definire i parametri JSON in un `dedicated-egress-ip-address.json` e fornito per curl tramite `... -d @./dedicated-egress-ip-address.json`.

[Scarica l&#39;esempio dedicato-egress-ip-address.json](./assets/dedicated-egress-ip-address.json)

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

   La firma HTTP della configurazione dell&#39;indirizzo IP in uscita dedicato è diversa solo dalla [porta di uscita flessibile](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) in quanto supporta anche l’opzione `nonProxyHosts` configurazione.

   `nonProxyHosts` dichiara un set di host per i quali la porta 80 o 443 deve essere instradata attraverso gli intervalli di indirizzi IP condivisi predefiniti anziché l&#39;IP di uscita dedicato. Questo può essere utile in quanto il traffico che attraversa IP condivisi può essere ulteriormente ottimizzato automaticamente tramite Adobe.

   Per ogni `portForwards` mappatura, la rete avanzata definisce la seguente regola di inoltro:

   | Host proxy | Porta proxy |  | Host esterno | Porta esterna |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Per ogni ambiente, convalida le regole di uscita effettive utilizzando l’API di Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) funzionamento.

   __richiesta HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Le configurazioni dell’indirizzo IP in uscita dedicato possono essere aggiornate utilizzando l’API di Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) funzionamento. Ricorda `enableEnvironmentAdvancedNetworkingConfiguration` è un `PUT` quindi tutte le regole devono essere fornite con ogni chiamata di questa operazione.

1. Ottieni il __indirizzo IP in uscita dedicato__ utilizzando un risolutore DNS (ad esempio [DNSChecker.org](https://dnschecker.org/)) sull’host: `p{programId}.external.adobeaemcloud.com`o `dig` dalla riga di comando.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   Nota che il nome host non può essere `pinged`, poiché si tratta di un&#39;uscita e _not_ e ingresso.

1. Ora puoi utilizzare l’indirizzo IP di uscita dedicato nel codice AEM e nella configurazione personalizzati. Spesso quando si utilizza un indirizzo IP in uscita dedicato, i servizi esterni a cui si AEM la connessione as a Cloud Service sono configurati per consentire solo il traffico da questo indirizzo IP dedicato.

## Connessione a servizi esterni tramite uscita porta dedicata

Con l’indirizzo IP in uscita dedicato abilitato, il codice AEM e la configurazione possono utilizzare l’IP in uscita dedicato per effettuare chiamate a servizi esterni. Esistono due tipi di chiamate esterne che AEM trattate in modo diverso:

1. Chiamate HTTP/HTTPS a servizi esterni su porte non standard
   + Include le chiamate HTTP/HTTPS effettuate a servizi in esecuzione su porte diverse dalle porte standard 80 o 443.
1. chiamate non HTTP/HTTPS a servizi esterni
   + Include tutte le chiamate non HTTP, come le connessioni con server di posta, database SQL o servizi che vengono eseguite su altri protocolli non HTTP/HTTPS.

Le richieste HTTP/HTTPS da AEM sulle porte standard (80/443) sono consentite per impostazione predefinita e non richiedono configurazioni o considerazioni aggiuntive.

>[!TIP]
>
> Consulta la documentazione dedicata sull&#39;indirizzo IP in uscita di AEM as a Cloud Service per [l&#39;intero insieme di regole di routing](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing).


### HTTP/HTTPS su porte non standard

Quando si creano connessioni HTTP/HTTPS a porte non standard (not-80/443) da AEM, la connessione deve essere effettuata tramite host e porte speciali, forniti tramite segnaposto.

AEM fornisce due set di variabili di sistema Java™ speciali associate a proxy HTTP/HTTPS AEM.

| Nome della variabile | Uso | Codice Java™ | Configurazione OSGi | | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Host proxy per connessioni HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | | `AEM_HTTP_PROXY_PORT` | Porta proxy per connessioni HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` | | `AEM_HTTPS_PROXY_HOST` | Host proxy per connessioni HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | | `AEM_HTTPS_PROXY_PORT` | Porta proxy per connessioni HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` |

Le richieste ai servizi esterni HTTP/HTTPS devono essere effettuate configurando la configurazione proxy del client Java™ HTTP utilizzando AEM valori host/porte proxy.

Quando si eseguono chiamate HTTP/HTTPS a servizi esterni su porte non standard, non corrisponde `portForwards` deve essere definito utilizzando l’API di Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` , in quanto le &quot;regole&quot; di inoltro della porta sono definite &quot;nel codice&quot;.

#### Esempi di codice

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS su porte non standard" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS su porte non standard</a></strong></div>
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