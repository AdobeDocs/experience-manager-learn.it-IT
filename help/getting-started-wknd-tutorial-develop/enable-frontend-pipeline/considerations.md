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
duration: 89
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Considerazioni sullo sviluppo

Dopo aver abilitato la pipeline front-end per distribuire solo le risorse front-end in un ambiente AEM as a Cloud Service, si verifica un certo impatto sullo sviluppo AEM locale e devi modificare il modello di ramificazione Git.

## Obiettivo

* Come avere un flusso di sviluppo front-end e back-end senza attriti
* Esaminare le dipendenze tra la pipeline full stack e front-end


## Considerazioni sullo sviluppo locale

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Approccio di sviluppo adattato

* Per lo sviluppo locale che utilizza l’SDK dell’AEM, il team di sviluppo back-end ha ancora bisogno della generazione clientlib tramite `ui.frontend` ma durante l’implementazione di Cloud Manager nell’ambiente as a Cloud Service AEM devi saltarlo. Questo pone una sfida su come isolare le modifiche di configurazione del progetto descritte nel [Aggiorna progetto](update-project.md) capitolo.

A __soluzione__ potrebbe essere necessario regolare il modello di ramificazione Git e assicurarsi che le modifiche alla configurazione del progetto AEM non tornino mai al __sviluppo locale__ diramare gli sviluppatori back-end AEM da utilizzare.


* Come parte di un miglioramento continuo del progetto AEM, se introduci nuovi componenti o aggiorni un componente esistente che presenta modifiche in entrambi `ui.app` e `ui.frontend` modulo, è necessario eseguire pipeline full stack e front-end.
