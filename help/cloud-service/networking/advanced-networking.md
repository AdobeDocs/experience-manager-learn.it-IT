---
title: Rete avanzata
description: Scopri AEM opzioni di rete avanzate di as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 3%

---

# Rete avanzata

AEM as a Cloud Service fornisce funzioni di rete avanzate che consentono una gestione precisa delle connessioni a e da AEM programmi as a Cloud Service.

|  | [Programmi di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programmi sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Supporta le reti avanzate | ↓ | ✘ |


AEM rete avanzata è composta da tre opzioni per la gestione della connettività con servizi esterni. Un programma Cloud Manager e i relativi ambienti as a Cloud Service AEM possono utilizzare un solo tipo di configurazione di rete avanzata alla volta, in modo da selezionare il tipo più appropriato.

|  | HTTP/HTTPS sulle porte standard | HTTP/HTTPS su porte non standard | Connessioni non HTTP/HTTPS | IP in uscita dedicato | Elenco &quot;Host non proxy&quot; | Connessione a servizi protetti da VPN | Limita il traffico AEM Publish per IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Nessuna rete avanzata__ | ↓ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Uscita porta flessibile__](./flexible-port-egress.md) | ↓ | ↓ | ↓ | ✘ | ✘ | ✘ | ✘ |
| [__Indirizzo IP in uscita dedicato__](./dedicated-egress-ip-address.md) | ↓ | ↓ | ↓ | ↓ | ↓ | ✘ | ✘ |
| [__Rete privata virtuale__](./vpn.md) | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ |


Per ulteriori dettagli sulle considerazioni relative alla selezione del tipo di rete avanzato appropriato, consulta [documentazione di rete avanzata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Esercitazioni di rete avanzate

Una volta identificata l’opzione di rete avanzata più appropriata in base alle esigenze della tua organizzazione, fai clic sull’esercitazione corrispondente qui sotto per visualizzare istruzioni ed esempi di codice dettagliati.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Uscita porta flessibile" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Uscita porta flessibile</a></strong></div>
      <p>
          Consenti traffico as a Cloud Service in uscita AEM su porte non standard.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Indirizzo IP di uscita dedicato del file" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Indirizzo IP in uscita dedicato</a></strong></div>
      <p>
        Originare il traffico in uscita AEM as a Cloud Service da un IP dedicato.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtual Private Network (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Virtual Private Network (VPN)</a></strong></div>
      <p>
        Proteggere il traffico tra un'infrastruttura cliente o fornitore e AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Esempi di codice

Questa raccolta fornisce esempi di configurazione e codice necessari per sfruttare le funzioni di rete avanzate per casi d’uso specifici.

Assicurati che [configurazione di rete avanzata](#advanced-networking) è stato impostato prima di seguire queste esercitazioni.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servizio e-mail</a></strong></div>
      <p>
        Esempio di configurazione OSGi tramite AEM per la connessione a servizi e-mail esterni.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS su porte non standard" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS su porte non standard</a></strong></div>
        <p>
            Esempio di codice Java™ per la connessione HTTP/HTTPS da AEM as a Cloud Service a un servizio esterno su porte HTTP/HTTPS non standard.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connessione SQL tramite JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connessione SQL tramite JDBC DataSourcePool</a></strong></div>
      <p>
            Esempio di codice Java™ che si collega a database SQL esterni configurando AEM pool di origini dati JDBC.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Connessione SQL tramite API Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Connessione SQL tramite API Java™</a></strong></div>
      <p>
            Esempio di codice Java™ per la connessione a database SQL esterni utilizzando le API SQL di Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Applicazione di un elenco consentiti IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Applicazione di un inserire nell'elenco Consentiti IP</a></strong></div>
      <p>
            Configura un inserire nell'elenco Consentiti IP in modo che solo il traffico VPN possa accedere a AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Restrizioni di accesso VPN basate sul percorso per AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Restrizioni di accesso VPN basate sul percorso per AEM Publish</a></strong></div>
      <p>
            Richiedi l’accesso VPN per percorsi specifici su AEM Publish.
      </p>
    </td>
</tr>
</table>
