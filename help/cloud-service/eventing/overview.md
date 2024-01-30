---
title: Eventi AEM
description: Scopri gli eventi AEM, cosa è, perché e quando usarlo ed esempi.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 573
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 0%

---


# Eventi AEM

Scopri gli eventi AEM, cosa è, perché e quando usarlo ed esempi.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

>[!IMPORTANT]
>
>L’evento AEM as a Cloud Service è disponibile solo per gli utenti registrati in modalità pre-release. Per abilitare gli eventi AEM nell’ambiente as a Cloud Service AEM, contattare [Squadra AEM-Eventing](mailto:grp-aem-events@adobe.com).

## Che cos’è

AEM Eventing è un sistema di gestione degli eventi nativo per il cloud che consente di abbonarsi agli eventi AEM per l’elaborazione in sistemi esterni. Un evento AEM è una notifica di modifica dello stato inviata dall’AEM ogni volta che si verifica un’azione specifica. Ad esempio, può includere eventi quando un frammento di contenuto viene creato, aggiornato o eliminato.

![Eventi AEM](./assets/aem-eventing.png)

Il diagramma precedente ha visualizzato il modo in cui AEM as a Cloud Service produce eventi e li invia all’Adobe I/O Eventi, che a sua volta li espone agli abbonati agli eventi.

In sintesi, i componenti principali sono tre:

1. **Provider di eventi:** AEM as a Cloud Service.
1. **Adobe I/O di eventi:** Piattaforma per sviluppatori per integrare, estendere e creare app ed esperienze basate sui prodotti e le tecnologie Adobe.
1. **Consumatore evento:** Sistemi di proprietà del cliente che si abbonano agli eventi AEM. Ad esempio, CRM (Customer Relationship Management), PIM (Product Information Management), OMS (Order Management System) o un&#39;applicazione personalizzata.

### Qual è la differenza?

Il [Evento Apache Sling](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), eventi OSGi e [Osservazione JCR](https://jackrabbit.apache.org/oak/docs/features/observation.html) tutti i meccanismi di offerta per abbonarsi ed elaborare gli eventi. Tuttavia, questi sono distinti dall’evento AEM discusso in questa documentazione.

Le distinzioni chiave degli eventi AEM comprendono:

- Il codice consumer dell’evento viene eseguito al di fuori dell’AEM, non nella stessa JVM dell’AEM.
- Il codice prodotto AEM è responsabile della definizione degli eventi e del loro invio agli Eventi Adobi I/O.
- Le informazioni sull’evento sono standardizzate e inviate in formato JSON. Per ulteriori informazioni, consulta [cloudevents](https://cloudevents.io/).
- Per comunicare nuovamente con l’AEM, il consumatore dell’evento utilizza l’API as a Cloud Service dell’AEM.


## Perché e quando usarlo

AEM Eventing offre numerosi vantaggi in termini di architettura del sistema ed efficienza operativa. I motivi principali per utilizzare l’evento AEM includono:

- **Per creare architetture basate su eventi**: facilita la creazione di sistemi liberamente accoppiati che possono essere scalati in modo indipendente e resistenti ai guasti.
- **Ridurre i costi operativi e il codice**: evita le personalizzazioni in AEM, che rendono i sistemi più facili da mantenere ed estendere, riducendo in tal modo i costi operativi.
- **Semplificare la comunicazione tra AEM e sistemi esterni**: elimina le connessioni point-to-point consentendo agli eventi Adobi I/O di gestire le comunicazioni, ad esempio determinando quali eventi AEM devono essere consegnati a sistemi o servizi specifici.
- **Maggiore durata degli eventi**: Adobe I/O Events è un sistema altamente disponibile e scalabile, progettato per gestire grandi volumi di eventi e distribuirli in modo affidabile agli abbonati.
- **Elaborazione parallela degli eventi**: consente la consegna di eventi a più abbonati contemporaneamente, consentendo l’elaborazione di eventi distribuiti tra vari sistemi.
- **Sviluppo di applicazioni senza server**: supporta la distribuzione del codice consumer dell’evento come applicazione senza server, migliorando ulteriormente la flessibilità e la scalabilità del sistema.

### Limitazioni

L&#39;AEM Eventing, pur essendo potente, ha alcune limitazioni da considerare:

- **Disponibilità limitata all’AEM as a Cloud Service**: Attualmente, AEM Eventing è disponibile esclusivamente per AEM as a Cloud Service.
- **Supporto limitato per eventi**: al momento, sono supportati solo gli eventi relativi ai frammenti di contenuto dell’AEM. Tuttavia, si prevede che il campo di applicazione si espanderà con l&#39;aggiunta di ulteriori eventi in futuro.

## Come abilitare

L’evento AEM è abilitato in base all’ambiente as a Cloud Service AEM ed è disponibile solo per gli ambienti in modalità pre-release. Contatto [Squadra AEM-Eventing](mailto:grp-aem-events@adobe.com) per abilitare l’ambiente AEM con AEM Eventing.

Se già abilitato, vedi [Abilitare gli eventi AEM nell’ambiente AEM Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) per i passaggi successivi.

## Iscrizione

Per iscriverti a AEM Events, non devi scrivere alcun codice nell’AEM, ma solo un [Console Adobe Developer](https://developer.adobe.com/) il progetto è configurato. La console Adobe Developer è un gateway per API, SDK, eventi, runtime e App Builder Adobe.

In questo caso, un _progetto_ nella console Adobe Developer consente di abbonarti agli eventi emessi dall’ambiente as a Cloud Service AEM e configurare la consegna degli eventi ai sistemi esterni.

Per ulteriori informazioni, consulta [Come abbonarsi a eventi AEM nella console Adobe Developer](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Come consumare

Esistono due metodi principali per consumare gli eventi AEM: _push_ e il _tirare_ metodo.

- **Metodo push**: in questo approccio, il consumatore dell’evento riceve una notifica proattiva da Adobe I/O Events quando un evento diventa disponibile. Le opzioni di integrazione includono Webhook, Adobe I/O Runtime e Amazon EventBridge.
- **Metodo pull**: qui, il consumatore dell’evento esegue attivamente il polling di Eventi di Adobe I/O per verificare la presenza di nuovi eventi. L’opzione di integrazione principale per questo metodo è l’API di Adobe Developer Journaling.

Per ulteriori informazioni, consulta [Elaborazione di eventi AEM tramite eventi Adobi I/O](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Esempi

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="Ricevere eventi AEM su un webhook" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">Ricevere eventi AEM su un webhook</a></strong></div>
        <p>
          Utilizza l’Adobe fornito dal webhook per ricevere gli eventi AEM e rivedere i dettagli dell’evento.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="Carica giornale di registrazione eventi AEM" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">Carica giornale di registrazione eventi AEM</a></strong></div>
        <p>
          Utilizza l’applicazione web fornita dall’Adobe per caricare gli eventi AEM dal giornale di registrazione e rivedere i dettagli dell’evento.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="Ricevi eventi AEM su azione Adobe I/O Runtime" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">Ricevi eventi AEM su azione Adobe I/O Runtime</a></strong></div>
        <p>
          Ricevi gli Eventi AEM e rivedi i dettagli dell’evento.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="Elaborazione di eventi AEM tramite Azione Adobe I/O Runtime" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div><strong><a href="./examples/event-processing-using-runtime-action.md">Elaborazione di eventi AEM tramite Azione Adobe I/O Runtime</a></strong></div>
        <p>
          Scopri come elaborare gli eventi AEM ricevuti utilizzando Azione Adobe I/O Runtime. L’elaborazione degli eventi include il callback dell’AEM, la persistenza dei dati dell’evento e la loro visualizzazione nell’SPA.
        </p>
      </td>
  </tr>    
</table>
