---
title: Configurare i modelli di risorse con  AEM Assets e  InDesign Server
description: I modelli di risorse consentono agli esperti di marketing di creare, gestire e distribuire risorse digitali per la stampa e la stampa. La creazione di brochure di marketing, biglietti da visita, volantini, annunci pubblicitari e cartoline postali è molto più semplice con Asset Templates (Modelli risorse) se integrata con  server InDesign. La configurazione di  server InDesign con AEM è trattata in questa sezione.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# Configurare i modelli di risorse con  AEM Assets e  InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

I modelli di risorse consentono agli esperti di marketing di creare, gestire e distribuire risorse digitali per la stampa e la stampa. La creazione di brochure di marketing, biglietti da visita, volantini, annunci pubblicitari e cartoline postali è molto più semplice con Asset Templates (Modelli risorse) se integrata con  server InDesign. La configurazione di  server InDesign con AEM è trattata in questa sezione.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **deve** essere connesso a un server di InDesign  in esecuzione quando il modello INDD viene caricato. Parte dell&#39;elaborazione iniziale sul file INDD richiede  server InDesign.

## Scarica  versione di prova InDesign Server {#download-indesign-server-trial}

Scarica [versione di prova InDesign Server download Sito Web](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## Avvio  InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
