---
title: React app con AEM Forms e Acrobat Sign
description: Acrobat Sign e AEM Forms consentono di automatizzare transazioni complesse e includere firme elettroniche legali come parte di un’esperienza digitale fluida.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 2ff7be5b-884c-420d-9a06-f0e2a99d3ef3
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# AEM Forms con Acrobat Sign Web Form


Questo tutorial illustra il caso d’uso della generazione di un documento di comunicazione interattivo con i dati inviati da [React](https://react.dev/) e la presentazione del documento generato per la firma tramite Acrobat Sign webform.

Di seguito è riportato il flusso del caso d’uso

* L’utente compila un modulo nell’app React.
* I dati del modulo vengono inviati a un endpoint AEM Forms per generare un documento di comunicazione interattivo.
* Crea un URL di widget Acrobat Sign utilizzando il documento generato.
* Presenta l’URL del widget all’applicazione chiamante affinché l’utente firmi il documento.

## Prerequisiti

Affinché il caso d’uso funzioni è necessario quanto segue:

* Un server AEM con pacchetto del componente aggiuntivo Forms
* Un [chiave di integrazione per un’applicazione Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## Passaggi successivi

Scrivi un [servizio OSGi personalizzato per generare documento di comunicazione interattivo](./create-ic-document.md) utilizzo di API documentate