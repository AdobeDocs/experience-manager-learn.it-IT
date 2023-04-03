---
title: Configurare i modelli di risorse con AEM Assets e InDesign Server
description: I modelli di risorse consentono agli addetti al marketing di creare, gestire e distribuire risorse digitali per scopi digitali e di stampa. La creazione di brochure, biglietti da visita, volantini, annunci pubblicitari e cartoline postali di marketing è molto più semplice grazie ai modelli di risorse integrati con il server InDesign. La configurazione del server InDesign con AEM è descritta in questa sezione.
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Configurare i modelli di risorse con AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

I modelli di risorse consentono agli addetti al marketing di creare, gestire e distribuire risorse digitali per scopi digitali e di stampa. La creazione di brochure, biglietti da visita, volantini, annunci pubblicitari e cartoline postali di marketing è molto più semplice grazie ai modelli di risorse integrati con il server InDesign. La configurazione del server InDesign con AEM è descritta in questa sezione.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **deve** essere connesso a un server InDesign in esecuzione al momento del caricamento del modello INDD. Parte dell’elaborazione iniziale sul file INDD richiede il server InDesign.

## Scarica la versione di prova di InDesign Server {#download-indesign-server-trial}

Scarica [Sito Web di download della versione di prova di InDesign Server](https://www.adobeprerelease.com/)

## Avvio di InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
