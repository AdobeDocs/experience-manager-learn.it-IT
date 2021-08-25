---
title: Predefiniti immagini
description: I predefiniti per immagini in Dynamic Media Classic contengono tutte le impostazioni necessarie per creare un’immagine con dimensioni, formato, qualità e nitidezza specifiche. I predefiniti per immagini sono un componente chiave del dimensionamento dinamico. Quando osservi un URL in Dynamic Media Classic, puoi facilmente vedere se è in uso un predefinito per immagini. Scopri i predefiniti per immagini, perché sono così utili e come crearne uno.
sub-product: dynamic-media
feature: Dynamic Media Classic, Image Presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---


# Predefiniti immagini {#image-presets}

Un predefinito per immagini è essenzialmente una ricetta che contiene tutte le impostazioni necessarie per creare un’immagine con dimensioni, formato, qualità e nitidezza specifiche. I predefiniti per immagini sono un componente chiave del dimensionamento dinamico.

Se osservi gli URL di quasi tutti i clienti di Dynamic Media Classic, probabilmente vedrai un predefinito per immagini in uso. Cerca $name$ alla fine dell’URL (con qualsiasi parola o parola sostituita da name).

I predefiniti per immagini accorciano l’URL; pertanto, invece di scrivere diverse istruzioni Image Serving per richiesta, puoi scrivere un singolo predefinito per immagini. Ad esempio, questi due URL producono la stessa immagine JPEG 300 x 300 con nitidezza, ma il secondo utilizza un predefinito immagine:

![immagine](assets/image-presets/image-preset-2.png)

Il vero valore di Image Preset è che l&#39;amministratore aziendale può aggiornare la definizione di tale Image Preset e influenzare ogni immagine che utilizza quel formato, senza modificare alcun codice web. Dopo la cancellazione della cache per l’URL, visualizzerai i risultati di qualsiasi modifica apportata a un predefinito immagine.

>[!IMPORTANT]
>
>Quando si ridimensiona un’immagine, le proporzioni, il rapporto tra la larghezza dell’immagine e l’altezza devono sempre essere proporzionali in modo che l’immagine non venga distorta.

Un predefinito per immagini ha un simbolo del dollaro ($) su entrambi i lati del suo nome e segue il punto interrogativo (?) separatore.

>[!TIP]
>
>Crea un predefinito per immagini per dimensione immagine univoca sul sito. Ad esempio, se hai bisogno di un&#39;immagine 350 X 350 per la pagina dei dettagli del prodotto, un&#39;immagine 120 X 120 per le pagine di ricerca e ricerca e un&#39;immagine 90 X 90 per un articolo di cross-selling/featured, allora hai bisogno di tre predefiniti immagine, che tu abbia 500 immagini o 500.00.

- Ulteriori informazioni su [Predefiniti immagini](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Scopri come [Creare un predefinito per immagini](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Predefiniti immagine e nitidezza

In genere, i predefiniti per immagini ridimensionano un’immagine e ogni volta che ne ridimensiona una dalle dimensioni originali, è necessario aggiungere nitidezza. Questo perché il ridimensionamento fa sì che molti pixel si uniscano e si fondano in uno spazio più piccolo, rendendo l&#39;immagine morbida e sfocata. La nitidezza aumenta il contrasto dei bordi e delle aree a contrasto elevato di un&#39;immagine.

Ci aspettiamo che le immagini ad alta risoluzione caricate in Dynamic Media Classic non necessitino di nitidezza quando visualizzate a dimensione piena — quando vengono ingrandite in. Tuttavia, con qualsiasi dimensione più piccola, una certa nitidezza è solitamente auspicabile.

>[!TIP]
>
>Nitidezza sempre durante il ridimensionamento delle immagini! Ciò significa che dovrai aggiungere nitidezza a ogni predefinito per immagini (e al predefinito per visualizzatori, di cui discuteremo più avanti).
>
>Se le tue immagini non sembrano buone, le possibilità sono che abbiano bisogno di nitidezza o forse la qualità era scarsa per cominciare.

Quanto nitidezza aggiungere è completamente soggettiva. Ad alcune persone piacciono le immagini più morbide, mentre ad altre piacciono molto nitide. È facile migliorare un&#39;immagine utilizzando una combinazione di filtri di nitidezza su un&#39;immagine. Tuttavia, è anche facile superare e rendere un&#39;immagine troppo nitida.

L’immagine seguente mostra tre livelli di nitidezza. Da destra a sinistra non si ha nitidezza, solo la quantità destra, e troppo.

![immagine](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic consente tre tipi di nitidezza: Nitidezza semplice, modalità di campionamento e Maschera definizione dettagli.

Ulteriori informazioni su [Opzioni di nitidezza di Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Risorse aggiuntive

[Guida ai predefiniti per immagini](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Impostazioni da utilizzare per ottimizzare la qualità delle immagini e la velocità di caricamento.

[L&#39;Immagine È Tutto Parte 2: Non è mai solo una sfocatura — Qualità e velocità](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Un post sul blog che parla dell&#39;utilizzo di Image Preset per la distribuzione di immagini di alta qualità e a caricamento rapido.
