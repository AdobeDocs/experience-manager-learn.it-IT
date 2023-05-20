---
title: Rete avanzata
description: Scopri le opzioni di rete avanzate di AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 3%

---

# Rete avanzata

AEM as a Cloud Service offre funzionalità di rete avanzate che consentono una gestione precisa delle connessioni da e verso i programmi as a Cloud Service AEM.

|  | [Programmi di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programmi sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Supporto di reti avanzate | ✔ | ✘ |


La rete avanzata AEM comprende tre opzioni per la gestione della connettività con i servizi esterni. Un programma Cloud Manager e i relativi ambienti AEM as a Cloud Service possono utilizzare solo un singolo tipo di configurazione di rete avanzata alla volta, in modo da assicurarsi che venga selezionato il tipo più appropriato.

|  | HTTP/HTTPS su porte standard | HTTP/HTTPS su porte non standard | connessioni non HTTP/HTTPS | IP in uscita dedicato | Elenco &quot;Host non proxy&quot; | Connessione a servizi protetti da VPN | Limita il traffico di pubblicazione AEM per IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Nessuna rete avanzata__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Uscita porta flessibile__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Indirizzo IP in uscita dedicato__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Virtual Private Network__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Per ulteriori dettagli sulle considerazioni da tenere in considerazione quando si seleziona il tipo di rete avanzato appropriato, vedere [documentazione di rete avanzata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Tutorial di rete avanzati

Una volta identificata l’opzione di rete avanzata più appropriata in base alle esigenze della tua organizzazione, fai clic sull’esercitazione corrispondente riportata di seguito per visualizzare istruzioni dettagliate ed esempi di codice.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Uscita porta flessibile" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Uscita porta flessibile</a></strong></div>
      <p>
          Consente il traffico AEM as a Cloud Service in uscita su porte non standard.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Indirizzo IP in uscita dedicato al file" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Indirizzo IP in uscita dedicato</a></strong></div>
      <p>
        Genera traffico AEM as a Cloud Service in uscita da un IP dedicato.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtual Private Network (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Virtual Private Network (VPN)</a></strong></div>
      <p>
        Traffico sicuro tra l'infrastruttura di un cliente o di un fornitore e l'AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Esempi di codice

Questa raccolta fornisce esempi della configurazione e del codice necessari per sfruttare funzioni di rete avanzate per casi d’uso specifici.

Assicurati che le [configurazione di rete avanzata](#advanced-networking) è stato impostato prima di seguire queste esercitazioni.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servizio di posta elettronica</a></strong></div>
      <p>
        Esempio di configurazione OSGi con AEM per la connessione a servizi di posta elettronica esterni.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Esempio di codice Java™ per rendere la connessione HTTP/HTTPS dall’AEM as a Cloud Service a un servizio esterno utilizzando il protocollo HTTP/HTTPS.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connessione SQL tramite DataSourcePool JDBC" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connessione SQL tramite DataSourcePool JDBC</a></strong></div>
      <p>
            Esempio di codice Java™ connessione a database SQL esterni tramite la configurazione del pool di origini dati JDBC dell'AEM.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Connessione SQL tramite API Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Connessione SQL tramite API Java™</a></strong></div>
      <p>
            Esempio di codice Java™ per la connessione a database SQL esterni tramite le API SQL di Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Applicazione di un elenco consentiti IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Applicazione di un elenco Consentiti IP di</a></strong></div>
      <p>
            Configurare un elenco Consentiti di accesso IP in modo che solo il traffico VPN possa accedere all’AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Limitazioni di accesso VPN basate su percorso per AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Limitazioni di accesso VPN basate su percorso per AEM Publish</a></strong></div>
      <p>
            Richiedi accesso VPN per percorsi specifici in AEM Publish.
      </p>
    </td>
</tr>
</table>
