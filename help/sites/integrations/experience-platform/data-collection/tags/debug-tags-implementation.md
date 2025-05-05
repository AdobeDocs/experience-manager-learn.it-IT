---
title: Debug di un’implementazione di tag
description: Introduzione ad alcuni strumenti e tecniche comuni per eseguire il debug di un’implementazione di tag. Scopri come utilizzare la Developer Console del browser e l’estensione Debugger di Experience Platform per identificare gli aspetti chiave di un’implementazione di Tag e risolvere problemi relativi a essi.
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
duration: 259
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# Debug di un’implementazione di tag {#debug-tags-implementation}

Introduzione agli strumenti e alle tecniche comuni utilizzati per eseguire il debug di un’implementazione di tag. Scopri come utilizzare la Developer Console del browser e l’estensione Debugger di Experience Platform per identificare gli aspetti chiave di un’implementazione di Tag e risolvere problemi relativi a essi.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Debug lato client tramite oggetto Satellite

Il debug lato client è utile per verificare il caricamento della regola della proprietà Tag o l’ordine di esecuzione. Ogni volta che una proprietà Tag viene aggiunta al sito Web, l&#39;oggetto JavaScript `_satellite` è presente nel browser per facilitare l&#39;evento lato client e il tracciamento dei dati.

Per abilitare il debug lato client, chiamare il metodo `setDebug(true)` sull&#39;oggetto `_satellite`.

1. Apri la console del browser ed esegui il comando seguente.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Ricarica la pagina del sito AEM e nel registro della console di verifica viene visualizzato il messaggio _regola attivata_ come riportato di seguito.

   ![Proprietà tag nelle pagine Author e Publish](assets/satellite-object-debugging.png)

## Debug tramite Adobe Experience Platform Debugger

Adobe fornisce l&#39;Adobe Experience Platform Debugger [estensione Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) per eseguire il debug, comprendere e ottenere informazioni approfondite sull&#39;integrazione.

1. Apri l’estensione Adobe Experience Platform Debugger e apri la pagina del sito nell’istanza Publish.

2. Nella sezione **Adobe Experience Platform Debugger > Riepilogo > Tag Adobe Experience Platform**, verifica i dettagli delle proprietà tag come Nome, Versione, Data build, Ambiente ed Estensioni.

   ![Dettagli proprietà Adobe Experience Platform Debugger e tag](assets/tag-property-details.png)

## Risorse aggiuntive {#additional-resources}

+ [Introduzione all&#39;Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=it)

+ [Riferimento oggetto satellite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html?lang=it)
