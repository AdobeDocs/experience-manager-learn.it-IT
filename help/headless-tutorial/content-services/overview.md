---
title: 'Guida introduttiva ad AEM Headless: Content Services'
description: Un tutorial end-to-end che illustra come creare ed esporre contenuti utilizzando AEM Headless.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 100%

---

# Guida introduttiva ad AEM Headless: Content Services

AEM Content Services sfrutta le tradizionali pagine AEM per comporre endpoint API REST headless; e i componenti AEM definiscono o fanno riferimento al contenuto da esporre in tali endpoint.

AEM Content Services consente di utilizzare le stesse astrazioni di contenuto utilizzate per la creazione di pagine web in AEM Sites, per definire il contenuto e gli schemi di queste API HTTP. L’utilizzo di pagine AEM e di componenti AEM consente ai marketer di comporre e aggiornare rapidamente API JSON flessibili in grado di alimentare qualsiasi applicazione.

## Tutorial su Content Services

Tutorial end-to-end che illustra come creare ed esporre contenuti tramite AEM e utilizzati da un’app mobile nativa, in uno scenario CMS headless.

>[!VIDEO](https://video.tv.adobe.com/v/329219?captions=ita&quality=12&learn=on)

Questo tutorial esplora come AEM Content Services può essere utilizzato per potenziare l’esperienza di un’app mobile che mostra informazioni su eventi (musica, spettacoli, arte, ecc.) curate dal team WKND.

Questo tutorial tratta i seguenti argomenti:

* Creare contenuti che rappresentano un evento utilizzando Frammenti di contenuto
* Definire endpoint di AEM Content Services utilizzando modelli e pagine di AEM Sites che espongono i dati dell’evento come JSON
* Utilizzare i componenti core WCM di AEM per consentire ai marketer di creare endpoint JSON
* Utilizzare JSON di AEM Content Services da un’app mobile
   * Viene utilizzato Android perché dispone di un emulatore multipiattaforma che permette a tutti gli utenti (Windows, macOS e Linux) di questo tutorial di eseguire l’app nativa.

## Progetto GitHub

Il codice sorgente e i pacchetti di contenuti sono disponibili in [AEM Guides: progetto GitHub WKND per dispositivi mobili](https://github.com/adobe/aem-guides-wknd-mobile).

Se riscontri un problema con l’esercitazione o con il codice, puoi segnalarlo mediante la funzione [Issues di GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Confronto tra AEM GraphQL e AEM Content Services

|                                | API GraphQL di AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definizione dello schema | Modelli per frammenti di contenuti strutturati | Componenti di AEM |
| Contenuto | Frammenti di contenuto | Componenti di AEM |
| Individuazione del contenuto | Per query basata su GraphQL | Per pagina AEM |
| Formato di consegna | JSON di GraphQL | JSON ComponentExporter di AEM |
