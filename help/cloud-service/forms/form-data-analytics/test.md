---
title: Report sui campi di dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per generare rapporti sui campi dei dati del modulo
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Verificare la soluzione

Visualizzare in anteprima e inviare il modulo utilizzando diverse combinazioni di valori modulo. Consenti a diversi o 30 minuti di visualizzare i dati nei rapporti di Adobe Analytics. I dati impostati per prop vengono visualizzati nel reporting prima dei dati impostati per eVar.

## Suite di rapporti

I dati del modulo acquisiti in Adobe Analytics vengono presentati in formato ad anello

**Invio per Stato**

![applicantsbystate](assets/donut.png)

Errori di convalida dei campi

![field-validation-error](assets/donut-field-validation.png)

## Debugging

Assicurati che il modulo adattivo utilizzi lo stesso contenitore di configurazione che contiene la configurazione Adobe Launch.

Per confermare che il modulo sta inviando dati ad Adobe Analytics, procedi come segue

* Apri gli Strumenti per sviluppatori nel browser.
* Immetti il seguente testo nel pannello Console.

```javascript
_satellite.setDebug(true)
```

Interagire con il modulo mantenendo aperta la finestra della console. Dovresti vedere qualcosa del genere

![console-debug](assets/debug.png)

## Utilizzare Adobe Experience Platform Debugger

Aggiungi il [Estensione AEP debugger](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) al browser (è necessario accedere) per ottenere ulteriori informazioni di debug

![platform-debugger](assets/platform-debugger.png)





