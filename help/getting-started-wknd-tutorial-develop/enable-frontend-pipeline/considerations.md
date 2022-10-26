---
title: Considerazioni sullo sviluppo
description: Considera l’impatto sul processo di sviluppo front-end e back-end una volta abilitata la pipeline front-end.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# Considerazioni sullo sviluppo

Dopo aver abilitato la pipeline front-end per distribuire solo le risorse front-end in AEM ambiente as a Cloud Service, si verifica un certo impatto sullo sviluppo AEM locale e devi modificare il modello di branching git.

## Obiettivo

* Come avere un flusso di sviluppo front-end e back-end senza attrito
* Esamina le dipendenze tra la pipeline full-stack e quella front-end


## Considerazioni sullo sviluppo locale

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## Approccio adattato allo sviluppo

* Per lo sviluppo locale tramite AEM SDK, il team di sviluppo back-end deve ancora generare clientlib tramite `ui.frontend` ma durante l’implementazione di Cloud Manager per AEM l’ambiente as a Cloud Service è necessario ignorarlo. Questo spiega come isolare le modifiche alla configurazione del progetto descritte in [Aggiorna progetto](update-project.md) capitolo.

A __soluzione__ potrebbe essere necessario regolare il modello di branching git e assicurarsi che le modifiche alla configurazione del progetto AEM non tornino mai più a __sviluppo locale__ gli sviluppatori di back-end AEM utilizzano .


* Come parte di un miglioramento continuo del progetto AEM, se introduci nuovi componenti o aggiorni un componente esistente che ha modifiche in entrambi `ui.app` e `ui.frontend` modulo, è necessario eseguire entrambe le pipeline complete e front-end.



