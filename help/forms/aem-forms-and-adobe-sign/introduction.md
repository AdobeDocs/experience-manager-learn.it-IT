---
title: Utilizzo di AEM Forms con Acrobat Sign
description: Acrobat Sign e AEM Forms consentono di automatizzare transazioni complesse e includere firme elettroniche legali come parte di un’esperienza digitale fluida.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Utilizzo di AEM Forms con Acrobat Sign

Acrobat Sign abilita i flussi di lavoro di firma elettronica per i moduli adattivi. Le firme elettroniche consentono di migliorare i flussi di lavoro per l&#39;elaborazione di documenti relativi a questioni legali, vendite, retribuzioni, gestione delle risorse umane e molte altre aree.
L’integrazione tra AEM Forms e Acrobat Sign consente di effettuare le seguenti operazioni

* Utilizza Adaptive Forms per acquisire dati e presentare documenti di record generati automaticamente (DoR) per le firme
* Crea un Forms adattivo in base al modello di PDF. Unisci i dati con il modello pdf e presentali anche per le firme
* Inviare documenti per la firma utilizzando il componente del flusso di lavoro Firma documento

## Prerequisiti

Per integrare Acrobat Sign con AEM Forms è necessario quanto segue:

* Un server AEM Forms abilitato per SSL
* Un account sviluppatore Acrobat Sign attivo.
* Un’applicazione API Acrobat Sign
* Credenziali (ID client e Segreto client) dell’applicazione API Acrobat Sign.
