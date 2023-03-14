---
title: Generare rapporti sui campi dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per creare rapporti sui campi dati dei moduli
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 4100061624bd8955bee392f1eced20f388f2902c
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Testare la soluzione

Consente di visualizzare in anteprima e inviare il modulo utilizzando diverse combinazioni di valori. Dai diversi ai 30 minuti per visualizzare i dati nei rapporti di Adobe Analytics. I dati impostati su prop vengono visualizzati nel reporting prima dei dati impostati su eVar.

## Suite di rapporti

I dati del modulo acquisiti in Adobe Analytics vengono presentati in formato ad anello Inviati per Stato

![applicantsbystate](assets/donut.png)

Errori di convalida campo

![field-validation-error](assets/donut-field-validation.png)

## Debugging

Assicurati che il modulo adattivo utilizzi lo stesso contenitore di configurazione che contiene l’Adobe Configurazione di Launch.

Per confermare che il modulo invia dati ad Adobe Analytics, effettua le seguenti operazioni

* Apri i Developer Tools nel browser.
* Immettete nel testo seguente nel pannello Console.

```javascript
_satellite.setDebug(true)
```

Interagisci con il modulo mantenendo aperta la finestra della console. Dovresti vedere qualcosa del genere

![console-debug](assets/debug.png)

## Utilizzare Adobe Experience Platform Debugger

Aggiungi il [Estensione debugger AEP](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) al browser (è necessario accedere) per ottenere ulteriori informazioni di debug

![platform-debugger](assets/platform-debugger.png)





