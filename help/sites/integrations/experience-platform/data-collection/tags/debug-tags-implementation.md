---
title: Debug di un’implementazione di tag
description: Introduzione ad alcuni strumenti e tecniche comuni per eseguire il debug di un’implementazione di tag. Scopri come utilizzare la Developer Console del browser e l’estensione Debugger di Experienci Platform per identificare gli aspetti chiave di un’implementazione di Tag e risolvere problemi relativi a essi.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 286
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 1%

---

# Debug di un’implementazione di tag {#debug-tags-implementation}

Introduzione agli strumenti e alle tecniche comuni utilizzati per eseguire il debug di un’implementazione di tag. Scopri come utilizzare la Developer Console del browser e l’estensione Debugger di Experienci Platform per identificare gli aspetti chiave di un’implementazione di Tag e risolvere problemi relativi a essi.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Debug lato client tramite oggetto Satellite

Il debug lato client è utile per verificare il caricamento della regola della proprietà Tag o l’ordine di esecuzione. Ogni volta che una proprietà Tag viene aggiunta al sito Web, il `_satellite` L’oggetto JavaScript è presente nel browser per facilitare l’evento lato client e il tracciamento dei dati.

Per abilitare il debug lato client, chiama il `setDebug(true)` metodo su `_satellite` oggetto.

1. Apri la console del browser ed esegui il comando seguente.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Ricarica la pagina del sito AEM e verifica la visualizzazione del registro della console _regola attivata_ messaggio simile al seguente.

   ![Proprietà Tag nelle pagine di authoring e pubblicazione](assets/satellite-object-debugging.png)

## Debug tramite Adobi Experience Platform Debugger

L’Adobe fornisce un Adobe Experience Platform Debugger [Estensione Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) e [Componente aggiuntivo Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) per eseguire il debug, comprendere e ottenere informazioni approfondite sull’integrazione.

1. Apri l’estensione Adobi Experience Platform Debugger e apri la pagina del sito nell’istanza Publish.

1. In **Adobe Experience Platform Debugger > Riepilogo > Tag Adobe Experience Platform** verificare i dettagli della proprietà Tag come Nome, Versione, Data build, Ambiente ed Estensioni.

   ![Dettagli proprietà Adobe Experience Platform Debugger e tag](assets/tag-property-details.png)

## Risorse aggiuntive {#additional-resources}

+ [Introduzione all’Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Riferimento a un oggetto satellite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
