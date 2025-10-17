---
title: Considerazioni sullo sviluppo
description: Una volta abilitata la pipeline front-end, prendi in considerazione l’impatto sul processo di sviluppo front-end e back-end.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '201'
ht-degree: 100%

---

# Considerazioni sullo sviluppo

Dopo aver abilitato la pipeline front-end per implementare solo le risorse front-end nell&#39;ambiente AEM as a Cloud Service, si verifica un certo impatto sullo sviluppo AEM locale e devi modificare il modello di ramificazione Git.

## Obiettivo

* Come avere un flusso di sviluppo front-end e back-end senza attriti
* Esaminare le dipendenze tra la pipeline full stack e front-end


## Considerazioni sullo sviluppo locale

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Approccio allo sviluppo adattato

* Per lo sviluppo locale con AEM SDK, il team di sviluppo back-end ha ancora bisogno della generazione clientlib tramite il modulo `ui.frontend`, ma durante l&#39;implementazione di Cloud Manager nell&#39;ambiente AEM as a Cloud Service devi saltarla. Questo rende difficile isolare le modifiche di configurazione del progetto descritte nel capitolo [Aggiornare il progetto](update-project.md).

Una __soluzione__ potrebbe essere quella di regolare il modello di diramazione git e di assicurarti che le modifiche alla configurazione del progetto AEM non tornino mai al ramo di __sviluppo locale__ utilizzato dagli sviluppatori back-end di AEM.


* Come parte di un miglioramento continuo del progetto AEM, se introduci nuovi componenti o aggiorni un componente esistente con modifiche sia nel modulo `ui.app` che nel modulo `ui.frontend`, devi eseguire entrambe le pipeline full stack e front-end.
