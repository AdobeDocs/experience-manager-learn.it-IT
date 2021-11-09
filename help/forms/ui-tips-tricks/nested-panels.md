---
title: Spostarsi sui pannelli nidificati
description: Spostarsi sui pannelli nidificati
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
source-git-commit: 84a0c78f89f78e161b460574b5927fc4aba2fe3a
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Schede di navigazione con più pannelli

Quando il modulo ha schede di navigazione a sinistra e se una delle schede dispone di più pannelli, è possibile nascondere il titolo dei pannelli secondari ed essere comunque in grado di spostarsi tra le schede e i pannelli secondari di tali schede

[Questa funzionalità è attiva nel modulo](https://forms.enablementadobe.com/content/forms/af/testnav1.html)




## Creare un modulo adattivo

Crea un modulo adattivo con la seguente struttura. Il pannello principale dispone di pannelli secondari visualizzati come schede a sinistra. Alcuni di questi &quot;**schede**&quot; dispongono di pannelli figlio aggiuntivi. Ad esempio, nella scheda Famiglia sono presenti due pannelli secondari denominati coniuge e figlio.
Una barra degli strumenti viene aggiunta anche sotto FormContainer con i pulsanti Prec e Successivo

![spaziatura barra degli strumenti](assets/multiple-panels.png)



Il comportamento predefinito di questo modulo consiste nel visualizzare tutti i pannelli a sinistra e quindi passare da una scheda all’altra facendo clic sul pulsante successivo.

Per modificare questo comportamento predefinito è necessario effettuare le seguenti operazioni

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


Aggiungi il codice seguente all’evento click del **Successivo** tramite l’editor di codice

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Aggiungi il codice seguente all’evento click del **Precedente** tramite l’editor di codice

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

Il codice di cui sopra ti aiuterà a navigare tra le schede e i pannelli secondari di ciascuna scheda.

## Nascondere il titolo dei pannelli figlio

Utilizza l’editor di stili per nascondere il titolo dei pannelli secondari delle schede.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
> La funzionalità descritta in questo articolo non funziona nell’ultima scheda. Ad esempio, se nella scheda Indirizzo erano presenti pannelli secondari, questa funzionalità non funzionerebbe.