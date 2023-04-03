---
title: Utilizzo di funzioni e editor di codice
description: Utilizzo di funzioni e editor di codice per creare regole di business
feature: Adaptive Forms
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Utilizzo di funzioni e editor di codice personalizzati {#using-functions-and-code-editor}

In questa parte utilizzeremo funzioni personalizzate e l’editor di codice per creare regole di business.

hai già installato [ClientLib con funzione personalizzata](assets/client-libs-and-logo.zip) precedente in questa esercitazione.

In genere una libreria client è costituita da file CSS e Javascript. Questa libreria client contiene un file javascript che espone una funzione per compilare i valori degli elenchi a discesa.


## Funzione per popolare elenco a discesa {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### Imposta il titolo del riepilogo del pannello {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### Pannello Convalida {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

Di seguito è riportato il codice utilizzato per convalidare i campi del pannello

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

È possibile rimuovere il commento alla riga 1 per eseguire il debug del codice nella finestra del browser.

Linea 4 - Ottieni il pannello corrente

Linea 5 - Convalida il pannello corrente.

Linea 9 - Se non si verificano errori, passare al pannello successivo

Visualizzare l’anteprima del modulo e verificare la nuova funzionalità abilitata.
