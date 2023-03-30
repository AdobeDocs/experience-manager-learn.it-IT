---
title: Debug dell’implementazione dei tag
description: Introduzione ad alcuni strumenti e tecniche comuni per eseguire il debug dell’implementazione dei tag. Scopri come utilizzare la Developer Console del browser e l’estensione Experience Platform Debugger per identificare gli aspetti chiave dell’implementazione dei tag e risolvere eventuali problemi relativi a essi.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Debug dell’implementazione dei tag {#debug-tags-implementation}

Introduzione agli strumenti e alle tecniche comuni utilizzati per eseguire il debug dell’implementazione dei tag. Scopri come utilizzare la Developer Console del browser e l’estensione Experience Platform Debugger per identificare gli aspetti chiave dell’implementazione dei tag e risolvere eventuali problemi relativi a essi.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Debug lato client tramite oggetto Satellite

Il debug lato client è utile per verificare il caricamento o l’ordine di esecuzione della regola della proprietà Tag. Ogni volta che una proprietà Tag viene aggiunta al sito web, il `_satellite` L&#39;oggetto JavaScript è presente nel browser per facilitare il tracciamento dei dati e degli eventi lato client.

Per abilitare il debug lato client, chiama il `setDebug(true)` metodo `_satellite` oggetto.

1. Apri la console del browser ed esegui il comando sottostante.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Ricarica la pagina del sito AEM e verifica le visualizzazioni del registro della console _regola attivata_ come sotto.

   ![Proprietà tag nelle pagine di authoring e pubblicazione](assets/satellite-object-debugging.png)

## Debug tramite Adobe Experience Platform Debugger

Adobe fornisce Adobe Experience Platform Debugger [Estensione Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) e [Componente aggiuntivo Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) per eseguire il debug, comprendere e ottenere informazioni sull’integrazione.

1. Apri l’estensione Adobe Experience Platform Debugger e apri la pagina del sito nell’istanza Publish

1. In **Adobe Experience Platform Debugger > Riepilogo > Tag Adobe Experience Platform** , verifica i dettagli della proprietà Tag quali Nome, Versione, Data build, Ambiente ed Estensioni.

   ![Dettagli sulle proprietà di Adobe Experience Platform Debugger e tag](assets/tag-property-details.png)

## Risorse aggiuntive {#additional-resources}

+ [Introduzione a Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Riferimento a oggetti satellitari](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
