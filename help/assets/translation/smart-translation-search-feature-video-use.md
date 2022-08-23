---
title: Utilizzo della ricerca di traduzioni avanzate con AEM Assets
description: La funzione di ricerca avanzata per la traduzione consente di eseguire automaticamente ricerche e individuare in più lingue AEM contenuti, sia Risorse che Pagine, supportando più di 50 lingue e riducendo la necessità di tradurre manualmente i contenuti.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Utilizzo della ricerca di traduzioni avanzate con AEM Assets{#using-smart-translation-search-with-aem-assets}

La funzione di ricerca avanzata per la traduzione consente di eseguire automaticamente ricerche e individuare in più lingue AEM contenuti, sia Risorse che Pagine, supportando più di 50 lingue e riducendo la necessità di tradurre manualmente i contenuti.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Ricerca di traduzione avanzata consente agli utenti di eseguire ricerche di contenuto in AEM utilizzando termini non inglesi, per far corrispondere le risorse in AEM che hanno termini inglesi equivalenti su di loro.

La Ricerca di traduzione avanzata è un complemento perfetto per AEM tag avanzati applicati alle risorse in inglese.

Questo video presuppone [Ricerca di traduzione intelligente AEM](smart-translation-search-technical-video-setup.md) è stato impostato.

## Funzionamento della ricerca di traduzioni intelligenti {#how-smart-translation-search-works}

![Diagramma del flusso di ricerca della traduzione intelligente](assets/smart-translation-search-flow.png)

1. AEM utente esegue una ricerca full-text, fornendo un termine di ricerca localizzato (ad esempio termine spagnolo &quot;uomo&quot;, &quot;hombre&quot;).
2. La Ricerca di traduzione intelligente, fornita dal bundle Apache Oak Machine Translation OSGi, è impegnata e valuta se i termini di ricerca forniti possono essere tradotti utilizzando i Language Pack registrati.
3. Vengono raccolti tutti i termini tradotti dal passaggio 2 e la query viene aumentata internamente per includerli come termini di ricerca. Questo set aumentato di termini di ricerca se valutato normalmente in base AEM indici di ricerca che individuano corrispondenze rilevanti.
4. I risultati della ricerca che corrispondono al termine originale (&#39;hombre&#39;) o al termine tradotto (&#39;man&#39;) vengono raccolti e restituiti all&#39;utente come risultati della ricerca.

## Risorse aggiuntive{#additional-resources}

* [Configurare la ricerca di traduzione avanzata con AEM Assets](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Pack](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
