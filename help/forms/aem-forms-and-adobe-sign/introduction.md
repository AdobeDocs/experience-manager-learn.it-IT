---
title: Utilizzo di AEM Forms con Adobe Sign
description: Adobe Sign e AEM Forms consentono di automatizzare transazioni complesse e includere firme elettroniche legali come parte di un’esperienza digitale senza soluzione di continuità.
feature: Adaptive Forms,Adobe Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Utilizzo di AEM Forms con Adobe Sign

Adobe Sign abilita i flussi di lavoro di firma elettronica per i moduli adattivi. Le firme elettroniche migliorano i flussi di lavoro per elaborare documenti per scopi legali, di vendita, di retribuzione, di gestione delle risorse umane e molte altre aree.
L’integrazione tra AEM Forms e Adobe Sign consente di effettuare le seguenti operazioni

* Utilizza Adaptive Forms per acquisire dati e presentare documenti di record generati automaticamente (DoR) per le firme
* Crea un Forms adattivo basato sul modello di PDF. Unire i dati con il modello pdf e presentare lo stesso per le firme
* Invia documenti per la firma tramite il componente del flusso di lavoro Firma documento

## Prerequisiti

Per integrare Adobe Sign con AEM Forms è necessario quanto segue:

* Un server AEM Forms abilitato per SSL
* Un account sviluppatore Adobe Sign attivo.
* Applicazione API Adobe Sign
* Credenziali (ID client e segreto client) dell’applicazione API Adobe Sign.
