---
title: Utilizzo di schede verticali nel Cloud Service AEM Forms
description: Creare un modulo adattivo utilizzando schede verticali
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Navigazione tra le schede

È possibile spostarsi tra le schede facendo clic sulle singole schede o utilizzando i pulsanti precedente e successivo del modulo.
Per spostarsi utilizzando i pulsanti, aggiungere due pulsanti al modulo e denominarli Precedente e Successivo. Associa la seguente funzione personalizzata all’evento click del pulsante per spostarti tra le schede.

Di seguito è riportata la funzione personalizzata che consente di spostarsi tra le schede.



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

Di seguito è riportato l’editor di regole per i pulsanti Successivo e Precedente

**Pulsante Avanti**

![pulsante successivo](assets/next-button.png)

**Pulsante Precedente**

![pulsante prec](assets/prev-button.png)