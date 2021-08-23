---
title: Configurare i modelli di risorse con AEM Assets e InDesign Server
description: I modelli di risorse consentono agli addetti al marketing di creare, gestire e distribuire risorse digitali per scopi digitali e di stampa. La creazione di brochure, biglietti da visita, volantini, annunci pubblicitari e cartoline postali di marketing è molto più semplice grazie ai modelli di risorse integrati con il server InDesign. La configurazione del server InDesign con AEM è descritta in questa sezione.
version: 6.3, 6.4, 6.5
topic: Gestione dei contenuti
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 1%

---


# Configurare i modelli di risorse con AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

I modelli di risorse consentono agli addetti al marketing di creare, gestire e distribuire risorse digitali per scopi digitali e di stampa. La creazione di brochure, biglietti da visita, volantini, annunci pubblicitari e cartoline postali di marketing è molto più semplice grazie ai modelli di risorse integrati con il server InDesign. La configurazione del server InDesign con AEM è descritta in questa sezione.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **deve** essere connesso a un server InDesign in esecuzione al momento del caricamento del modello INDD. Parte dell’elaborazione iniziale sul file INDD richiede il server InDesign.

## Scarica la versione di prova di InDesign Server {#download-indesign-server-trial}

Scarica [InDesign Server trial download Website](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## Avvio di InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
