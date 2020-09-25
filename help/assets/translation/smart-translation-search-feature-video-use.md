---
title: Utilizzo di Smart Translation Search con  AEM Assets
seo-title: Utilizzo di Smart Translation Search con  AEM Assets
description: Smart Translation Search consente di effettuare ricerche e individuare automaticamente in più lingue tra AEM contenuto, sia Risorse che Pagine, supportando più di 50 lingue e riducendo la necessità di tradurre manualmente i contenuti.
seo-description: Smart Translation Search consente di effettuare ricerche e individuare automaticamente in più lingue tra AEM contenuto, sia Risorse che Pagine, supportando più di 50 lingue e riducendo la necessità di tradurre manualmente i contenuti.
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Utilizzo di Smart Translation Search con  AEM Assets{#using-smart-translation-search-with-aem-assets}

Smart Translation Search consente di effettuare ricerche e individuare automaticamente in più lingue tra AEM contenuto, sia Risorse che Pagine, supportando più di 50 lingue e riducendo la necessità di tradurre manualmente i contenuti.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Smart Translation Search consente agli utenti di eseguire ricerche di contenuto in AEM utilizzando termini non inglesi, per far corrispondere le risorse in AEM con termini equivalenti in inglese.

Smart Translation Search è un complimento perfetto per AEM Smart Tags che vengono applicati alle risorse in inglese.

Questo video presuppone che [AEM sia stata configurata la ricerca](smart-translation-search-technical-video-setup.md) di traduzione intelligente.

## Come funziona la ricerca di traduzione intelligente {#how-smart-translation-search-works}

![Diagramma flusso ricerca traduzione intelligente](assets/smart-translation-search-flow.png)

1. AEM utente esegue una ricerca full-text, fornendo un termine di ricerca localizzato (ad esempio il termine spagnolo &quot;uomo&quot;, &quot;hombre&quot;).
2. La ricerca di traduzione intelligente, fornita dal pacchetto Apache Oak Machine Translation OSGi, è impegnata e valuta se i termini di ricerca forniti possono essere tradotti utilizzando i Language Pack registrati.
3. Tutti i termini tradotti del passaggio 2 vengono raccolti e la query viene aumentata internamente per includerli come termini di ricerca. Questo set di termini di ricerca aumentato se valutato normalmente rispetto AEM indici di ricerca che individuano corrispondenze rilevanti.
4. I risultati della ricerca che corrispondono al termine originale (&#39;hombre&#39;) o al termine tradotto (&#39;man&#39;) vengono raccolti e restituiti all&#39;utente come risultati della ricerca.

## Risorse aggiuntive{#additional-resources}

* [Configurare la ricerca per la traduzione intelligente con  AEM Assets](smart-translation-search-technical-video-setup.md)
* [Pacchetti linguistici Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)