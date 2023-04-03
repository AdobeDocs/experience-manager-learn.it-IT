---
title: Guida introduttiva a AEM Headless - Content Services
description: Un tutorial completo che illustra come creare ed esporre contenuti utilizzando AEM Headless.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 5aa32791-861a-48e3-913c-36028373b788
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 6%

---

# Guida introduttiva ad AEM Headless - Content Services

AEM Content Services sfrutta le pagine AEM tradizionali per comporre endpoint API REST headless e AEM componenti definiscono o fanno riferimento al contenuto da esporre su questi endpoint.

AEM Content Services consente di utilizzare le stesse astrazioni di contenuto utilizzate per creare pagine web in AEM Sites, per definire il contenuto e gli schemi di queste API HTTP. L’utilizzo di pagine AEM e componenti AEM consente agli addetti al marketing di comporre e aggiornare rapidamente API JSON flessibili che possono alimentare qualsiasi applicazione.

## Tutorial su Content Services

Un tutorial end-to-end che illustra come creare ed esporre contenuti utilizzando AEM e utilizzati da un’app mobile nativa, in uno scenario CMS headless.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Questa esercitazione esplora come AEM Content Services può essere utilizzato per sviluppare l&#39;esperienza di un&#39;app mobile che visualizza informazioni sull&#39;evento (musica, prestazioni, arte, ecc.) che è curato dal team WKND.

Questa esercitazione tratterà i seguenti argomenti:

* Creare contenuti che rappresentano un evento utilizzando frammenti di contenuto
* Definire un punto finale AEM Content Services utilizzando i modelli e le pagine di AEM Sites che espongono i dati evento come JSON
* Scopri come AEM i componenti core WCM possono essere utilizzati per consentire agli esperti di marketing di creare endpoint JSON
* Utilizzare AEM JSON di Content Services da un’app mobile
   * L&#39;utilizzo di Android è dovuto al fatto che dispone di un emulatore multipiattaforma che tutti gli utenti (Windows, macOS e Linux) di questa esercitazione possono utilizzare per eseguire l&#39;app nativa.

## Progetto GitHub

Il codice sorgente e i pacchetti di contenuto sono disponibili nel [Guide AEM - Progetto GitHub WKND per dispositivi mobili](https://github.com/adobe/aem-guides-wknd-mobile).

Se trovi un problema con l&#39;esercitazione o il codice, lascia un [Problema GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL e servizi di contenuti AEM

|  | API di GraphQL AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definizione dello schema | Modelli per frammenti di contenuto strutturati | Componenti AEM |
| Contenuto | Frammenti di contenuto | Componenti AEM |
| Ricerca dei contenuti | Query per GraphQL | Per AEM pagina |
| Formato di consegna | JSON GraphQL | JSON AEM ComponentExporter |
