---
title: Utilizzo delle funzioni e dell’editor di codice
description: Utilizzo di funzioni ed editor di codice per l’authoring di regole business
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
duration: 460
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Utilizzo di funzioni personalizzate ed editor di codice {#using-functions-and-code-editor}

In questa parte utilizzeremo le funzioni personalizzate e l’editor di codice per creare regole aziendali.

hai già installato [ClientLib con funzione personalizzata](assets/client-libs-and-logo.zip) in questa esercitazione.

In genere, una libreria client è costituita da file CSS e JavaScript. Questa libreria client contiene un file JavaScript che espone una funzione per popolare i valori dell’elenco a discesa.


## Funzione per popolare l&#39;elenco a discesa {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/326882?quality=12&learn=on&captions=ita)

### Imposta il titolo di riepilogo del pannello {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/329221?quality=12&learn=on&captions=ita)

#### Convalida pannello {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/329220?quality=12&learn=on&captions=ita)

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

Puoi rimuovere il commento dalla riga 1 per eseguire il debug del codice nella finestra del browser.

Riga 4: visualizza il pannello corrente

Riga 5 - Convalida il pannello corrente.

Riga 9 - Se non si verificano errori, passa al pannello successivo

Visualizzare l&#39;anteprima del modulo e verificare la funzionalità appena attivata.
