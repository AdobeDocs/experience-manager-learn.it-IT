---
title: Utilizzo di funzioni e editor di codice
seo-title: Utilizzo di funzioni e editor di codice
description: Utilizzo di funzioni e editor di codice per creare regole di business
seo-description: Utilizzo di funzioni e editor di codice per creare regole di business
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: moduli adattivi
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---


# Utilizzo di funzioni personalizzate e editor di codice {#using-functions-and-code-editor}

In questa parte utilizzeremo funzioni personalizzate e l’editor di codice per creare regole di business.

hai già installato [ClientLib con funzione personalizzata](assets/client-libs-and-logo.zip) in precedenza in questa esercitazione.

In genere una libreria client è costituita da file CSS e Javascript. Questa libreria client contiene un file javascript che espone una funzione per compilare i valori degli elenchi a discesa.


## Funzione per popolare elenco a discesa {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Imposta il titolo di riepilogo del pannello {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Convalida pannello {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

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
