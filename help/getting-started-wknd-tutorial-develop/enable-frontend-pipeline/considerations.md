---
title: Considerazioni sullo sviluppo
description: Considera l’impatto sul processo di sviluppo front-end e back-end una volta abilitata la pipeline front-end.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 79
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Considerazioni sullo sviluppo

Dopo aver abilitato la pipeline front-end per distribuire solo le risorse front-end nell’ambiente AEM as a Cloud Service, si verifica un certo impatto sullo sviluppo AEM locale e devi modificare il modello di ramificazione Git.

## Obiettivo

* Come avere un flusso di sviluppo front-end e back-end senza attriti
* Esaminare le dipendenze tra la pipeline full stack e front-end


## Considerazioni sullo sviluppo locale

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Approccio di sviluppo adattato

* Per lo sviluppo locale che utilizza l’SDK dell’AEM, il team di sviluppo back-end ha ancora bisogno della generazione clientlib tramite il modulo `ui.frontend`, ma durante l’implementazione di Cloud Manager nell’ambiente AEM as a Cloud Service devi saltarla. Questo rende difficile isolare le modifiche di configurazione del progetto descritte nel capitolo [Aggiorna progetto](update-project.md).

Una __soluzione__ potrebbe essere quella di regolare il modello di diramazione Git e di assicurarsi che le modifiche alla configurazione del progetto AEM non rifluiscano mai nel ramo di __sviluppo locale__ utilizzato dagli sviluppatori back-end AEM.


* Come parte di un miglioramento continuo del progetto AEM, se introduci nuovi componenti o aggiorni un componente esistente con modifiche sia nel modulo `ui.app` che nel modulo `ui.frontend`, devi eseguire entrambe le pipeline full stack e front-end.
