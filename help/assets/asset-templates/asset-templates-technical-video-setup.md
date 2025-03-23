---
title: Configurare modelli di risorse con AEM Assets e InDesign Server
description: I modelli di risorse consentono agli addetti al marketing di creare, gestire e distribuire risorse digitali sia per la stampa che per quella digitale. La creazione di brochure di marketing, biglietti da visita, volantini, annunci e cartoline è molto più semplice con i modelli di risorse se integrati con il server InDesign. La configurazione del server InDesign con AEM è descritta in questa sezione.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Configurare modelli di risorse con AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

I modelli di risorse consentono agli addetti al marketing di creare, gestire e distribuire risorse digitali sia per la stampa che per quella digitale. La creazione di brochure di marketing, biglietti da visita, volantini, annunci e cartoline è molto più semplice con i modelli di risorse se integrati con il server InDesign. La configurazione del server InDesign con AEM è descritta in questa sezione.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **deve** essere connesso a un server InDesign in esecuzione quando viene caricato il modello INDD. Parte dell’elaborazione iniziale sul file INDD richiede il server InDesign.

## Scarica la versione di prova di InDesign Server {#download-indesign-server-trial}

Scarica [sito Web di download della versione di prova di InDesign Server](https://www.adobeprerelease.com/)

## Avvio di InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
