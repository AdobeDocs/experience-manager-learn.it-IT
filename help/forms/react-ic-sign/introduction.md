---
title: Reagire app con AEM Forms e Acrobat Sign
description: Acrobat Sign e AEM Forms consentono di automatizzare transazioni complesse e includere firme elettroniche legali come parte di un’esperienza digitale senza soluzione di continuità.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# AEM Forms con Acrobat Sign Web Form


Questa esercitazione illustra il caso di utilizzo per la generazione di un documento di comunicazione interattivo con i dati inviati dalla [Reagire](https://react.dev/) app e presentazione del documento generato per la firma tramite il modulo web Acrobat Sign.

Di seguito è riportato il flusso del caso d’uso

* L’utente compila un modulo nell’app React.
* I dati del modulo vengono inviati a un endpoint AEM Forms per la generazione di documenti di comunicazione interattivi.
* Crea l&#39;url del widget Acrobat Sign utilizzando il documento generato.
* Presenta l&#39;url del widget all&#39;applicazione chiamante affinché l&#39;utente firmi il documento.

## Prerequisiti

Affinché il caso d’uso funzioni è necessario quanto segue:

* Un server AEM con pacchetto aggiuntivo Forms
* Un [chiave di integrazione per un’applicazione Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

