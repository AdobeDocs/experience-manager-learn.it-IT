---
title: Guida introduttiva di Headless AEM - Content Services
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

AEM Content Services sfrutta le pagine AEM tradizionali per comporre endpoint API REST headless; i componenti AEM definiscono o fanno riferimento al contenuto da esporre su tali endpoint.

AEM Content Services consente di utilizzare le stesse astrazioni di contenuto utilizzate per l’authoring di pagine web in AEM Sites, per definire il contenuto e gli schemi di queste API HTTP. L’utilizzo delle pagine AEM e dei componenti AEM consente agli addetti al marketing di comporre e aggiornare rapidamente API JSON flessibili in grado di alimentare qualsiasi applicazione.

## Tutorial su Content Services

Un tutorial end-to-end che illustra come creare ed esporre contenuti utilizzando l’AEM e utilizzati da un’app mobile nativa, in uno scenario CMS headless.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Questo tutorial esplora come utilizzare i servizi di contenuti AEM per potenziare l’esperienza di un’app mobile che visualizza informazioni sugli eventi (musica, prestazioni, immagini, ecc.) curata dal team WKND.

Questo tutorial tratta i seguenti argomenti:

* Creare contenuti che rappresentano un evento utilizzando Frammenti di contenuto
* Definire un endpoint di AEM Content Services utilizzando i modelli e le pagine di AEM Sites che espongono i dati dell’evento come JSON
* Scopri come utilizzare i componenti core WCM dell’AEM per consentire agli addetti al marketing di creare endpoint JSON
* Utilizzare JSON AEM Content Services da un’app mobile
   * Android viene utilizzato perché dispone di un emulatore multipiattaforma che tutti gli utenti (Windows, macOS e Linux) di questo tutorial possono utilizzare per eseguire l’app nativa.

## Progetto GitHub

Il codice sorgente e i pacchetti di contenuti sono disponibili nel [Guide AEM - Progetto WKND Mobile GitHub](https://github.com/adobe/aem-guides-wknd-mobile).

Se riscontri un problema con l’esercitazione o con il codice, lascia un [Problema GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## GraphQL AEM e servizi di contenuti AEM

|  | API GraphQL dell’AEM | Servizi di contenuti AEM |
|--------------------------------|:-----------------|:---------------------|
| Definizione schema | Modelli per frammenti di contenuto strutturati | Componenti AEM |
| Contenuto | Frammenti di contenuto | Componenti AEM |
| Individuazione dei contenuti | Query basata su GraphQL | Per pagina AEM |
| Formato di consegna | JSON GRAPHQL | JSON per l’esportazione di componenti AEM |
