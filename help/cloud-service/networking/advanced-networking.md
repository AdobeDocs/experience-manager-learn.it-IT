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
source-git-commit: 6e7130cd98700bdb5e7f330ca0506fe89ea0eb94
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 4%

---

# Rete avanzata

AEM as a Cloud Service fornisce funzioni di rete avanzate che consentono una gestione precisa delle connessioni a e da AEM programmi as a Cloud Service.

|  | [Programmi di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programmi sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------|
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
