---
title: Generare rapporti sui campi dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per creare rapporti sui campi dati dei moduli
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# Testare la soluzione

Consente di visualizzare in anteprima e inviare il modulo utilizzando diverse combinazioni di valori. Dai diversi ai 30 minuti per visualizzare i dati nei rapporti di Adobe Analytics. I dati impostati su prop vengono visualizzati nel reporting prima dei dati impostati su eVar.

## Suite per report

I dati del modulo acquisiti in Adobe Analytics vengono presentati in formato ad anello

**Invii per stato**

![applicantsbystate](assets/donut.png)

Errori di convalida campo

![errore-convalida-campo](assets/donut-field-validation.png)

## Debugging

Assicurati che il modulo adattivo utilizzi lo stesso contenitore di configurazione che contiene la configurazione di Adobe Launch.

Per confermare che il modulo invia dati ad Adobe Analytics, effettua le seguenti operazioni

* Apri i Developer Tools nel browser.
* Immettete nel testo seguente nel pannello Console.

```javascript
_satellite.setDebug(true)
```

Interagisci con il modulo mantenendo aperta la finestra della console. Dovresti vedere qualcosa del genere

![console-debug](assets/debug.png)

## Utilizzare Adobe Experience Platform Debugger

Aggiungi l&#39;estensione [AEP Debugger](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) al browser (è necessario effettuare l&#39;accesso) per ottenere ulteriori informazioni di debug

![platform-debugger](assets/platform-debugger.png)

## Complimenti

Hai integrato correttamente AEM Forms as a Cloud Service con Adobe Analytics per creare rapporti sui campi dati dei moduli.