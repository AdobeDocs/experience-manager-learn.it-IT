---
title: Utilizzo di Smart Translation Search con AEM Assets
description: Smart Translation Search consente la ricerca e il rilevamento automatici in più lingue dei contenuti AEM, sia per le risorse che per le pagine, supportando più di 50 lingue e riducendo la necessità di traduzione manuale dei contenuti.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Utilizzo di Smart Translation Search con AEM Assets{#using-smart-translation-search-with-aem-assets}

Smart Translation Search consente la ricerca e il rilevamento automatici in più lingue dei contenuti AEM, sia per le risorse che per le pagine, supportando più di 50 lingue e riducendo la necessità di traduzione manuale dei contenuti.

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

La funzione di ricerca intelligente delle traduzioni dell’AEM consente agli utenti di eseguire ricerche di contenuti nell’AEM utilizzando termini non inglesi, in modo che corrispondano alle risorse dell’AEM che contengono termini inglesi equivalenti.

Smart Translation Search è un complimento perfetto per i tag avanzati AEM che vengono applicati alle risorse in inglese.

Questo video presuppone [Ricerca di traduzione intelligente AEM](smart-translation-search-technical-video-setup.md) è stato configurato.

## Come funziona la ricerca intelligente delle traduzioni {#how-smart-translation-search-works}

![Diagramma del flusso di ricerca di traduzione intelligente](assets/smart-translation-search-flow.png)

1. L&#39;utente AEM esegue una ricerca full-text, fornendo un termine di ricerca localizzato (es. il termine spagnolo per &quot;uomo&quot;, &quot;hombre&quot;).
2. Viene attivata la Ricerca avanzata di traduzione, fornita dal bundle OSGi Apache Oak Machine Translation, e valuta se i termini di ricerca forniti possono essere tradotti utilizzando i Language Pack registrati.
3. Tutti i termini tradotti dal passaggio #2 vengono raccolti e la query viene incrementata internamente per includerli come termini di ricerca. Questo set aumentato di termini di ricerca se valutato normalmente rispetto agli indici di ricerca AEM che individuano corrispondenze rilevanti.
4. I risultati della ricerca che corrispondono al termine originale (&#39;hombre&#39;) o al termine tradotto (&#39;man&#39;) vengono raccolti e restituiti all&#39;utente come risultati della ricerca.

## Risorse aggiuntive{#additional-resources}

* [Configurare la ricerca di traduzione intelligente con AEM Assets](smart-translation-search-technical-video-setup.md)
* [Language Pack di Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
