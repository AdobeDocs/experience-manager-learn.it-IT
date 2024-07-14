---
title: Passare ai pannelli nidificati
description: Passare ai pannelli nidificati
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 264
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# Schede di navigazione con più pannelli

Se nel modulo sono presenti schede di navigazione a sinistra e una delle schede è costituita da più pannelli, è possibile nascondere il titolo dei pannelli figlio e continuare a spostarsi tra le schede e i pannelli figlio di tali schede

## Creare un modulo adattivo

Crea un modulo adattivo con la seguente struttura. Il pannello principale ha pannelli secondari che vengono visualizzati come schede a sinistra. Alcune di queste &quot;**schede**&quot; contengono pannelli figlio aggiuntivi. Ad esempio, la scheda Famiglia include due pannelli figlio denominati Coniuge e Figli.

Viene aggiunta anche una barra degli strumenti sotto FormContainer con i pulsanti Prec e Next

![spaziatura barra degli strumenti](assets/multiple-panels.png)



Il comportamento predefinito di questo modulo consiste nel visualizzare tutti i pannelli a sinistra e quindi passare da una scheda all&#39;altra facendo clic sul pulsante successivo.

Per modificare questo comportamento predefinito, è necessario effettuare le seguenti operazioni

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=12&learn=on)


Aggiungi il codice seguente all&#39;evento click del pulsante **Next** utilizzando l&#39;editor di codice

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Aggiungi il codice seguente all&#39;evento click del pulsante **Prev** utilizzando l&#39;editor di codice

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

Il codice riportato sopra consente di spostarsi tra le schede e i pannelli secondari di ciascuna scheda.

## Nascondi il titolo dei pannelli figlio

Utilizza l’editor di stili per nascondere il titolo delle schede nei pannelli secondari.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=12&learn=on)

>[!NOTE]
>
>La funzionalità descritta in questo articolo non funziona nell’ultima scheda. Ad esempio, se la scheda Indirizzo avesse pannelli secondari, questa funzionalità non funzionerebbe.
