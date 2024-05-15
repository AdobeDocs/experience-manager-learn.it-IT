---
title: Predefiniti immagini
description: I predefiniti per immagini in Dynamic Media Classic contengono tutte le impostazioni necessarie per creare un'immagine con dimensioni, formato, qualità e nitidezza specifici. I predefiniti per immagini sono un componente chiave del dimensionamento dinamico. Quando osservi un URL in Dynamic Media Classic, puoi facilmente vedere se è in uso un predefinito immagine. Scopri i predefiniti per immagini, perché sono così utili e come crearne uno.
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 1%

---

# Predefiniti immagini {#image-presets}

Un predefinito per immagini è essenzialmente una ricetta che contiene tutte le impostazioni necessarie per creare un’immagine con dimensioni, formato, qualità e nitidezza specifici. I predefiniti per immagini sono un componente chiave del dimensionamento dinamico.

Se osservi gli URL di quasi tutti i clienti Dynamic Media Classic, probabilmente vedrai un predefinito immagine in uso. È sufficiente cercare $name$ alla fine dell&#39;URL (con una o più parole sostituite da name).

I predefiniti immagine abbreviano l’URL, pertanto invece di scrivere diverse istruzioni Image Server per richiesta, puoi scrivere un singolo predefinito immagine. Ad esempio, questi due URL producono la stessa immagine di 300 x 300 JPEG con nitidezza, ma il secondo utilizza un predefinito immagine:

![immagine](assets/image-presets/image-preset-2.png)

Il vero valore dei predefiniti immagine è che qualsiasi amministratore aziendale può aggiornare la definizione di tale predefinito e influire su ogni immagine che utilizza quel formato, senza modificare alcun codice web. Una volta cancellata la cache per l’URL, verranno visualizzati i risultati di eventuali modifiche a un predefinito immagine.

>[!IMPORTANT]
>
>Quando si ridimensiona un’immagine, le proporzioni e il rapporto tra la larghezza e l’altezza dell’immagine devono sempre essere proporzionati, in modo che l’immagine non venga distorta.

Un predefinito immagine presenta un simbolo di dollaro ($) su entrambi i lati del nome e segue il punto interrogativo (?) separatore.

>[!TIP]
>
>Crea un predefinito immagine per ogni dimensione immagine univoca sul sito. Ad esempio, se hai bisogno di un’immagine 350 X 350 per la pagina dei dettagli del prodotto, un’immagine 120 X 120 per le pagine di ricerca e navigazione e un’immagine 90 X 90 per un articolo in evidenza o cross-selling, allora hai bisogno di tre predefiniti immagine, che tu abbia 500 immagini o 500.000.

- Ulteriori informazioni su [Predefiniti immagine](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Scopri come [Creare un predefinito immagine](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Predefiniti immagine e nitidezza

In genere, i predefiniti per immagini ridimensionano un’immagine e, ogni volta che questa viene ridimensionata dalle dimensioni originali, è necessario aggiungere nitidezza. Questo perché il ridimensionamento fa sì che molti pixel si fondano e si fondano in uno spazio più piccolo, rendendo l&#39;immagine morbida e sfocata. La nitidezza aumenta il contrasto dei bordi e delle aree ad alto contrasto di un&#39;immagine.

Ci aspettiamo che le immagini ad alta risoluzione caricate in Dynamic Media Classic non richiedano nitidezza quando visualizzate a dimensione intera — quando ingrandite. Tuttavia, per le dimensioni più piccole, in genere è auspicabile una certa nitidezza.

>[!TIP]
>
>Aumenta sempre la nitidezza durante il ridimensionamento delle immagini. Ciò significa che dovrai aggiungere nitidezza a ogni predefinito immagine (e predefinito visualizzatore, di cui parleremo più avanti).
>
>Se le immagini non sono di buona qualità, è probabile che lo siano perché hanno bisogno di nitidezza o forse la qualità è stata scarsa per iniziare.

Quanto nitidezza aggiungere è del tutto soggettivo. Ad alcune persone piacciono le immagini più morbide, mentre ad altre piacciono molto nitide. È facile migliorare un’immagine applicando una combinazione di filtri di nitidezza. Tuttavia, è anche facile esagerare e rendere l&#39;immagine troppo nitida.

L&#39;immagine seguente mostra tre livelli di nitidezza. Da destra a sinistra non si ha la nitidezza, solo la quantità giusta, e troppo.

![immagine](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic consente tre tipi di nitidezza: Nitidezza semplice, Modalità ricampionamento e Maschera definizione dettagli.

Ulteriori informazioni su [Opzioni nitidezza Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Risorse aggiuntive

[Guida ai predefiniti per immagini](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Impostazioni da utilizzare per ottimizzare la qualità delle immagini e la velocità di caricamento.

[L&#39;immagine è tutto parte 2: non è mai solo una sfocatura — qualità contro velocità](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Post di un blog che illustra come utilizzare i predefiniti per immagini per offrire immagini di alta qualità e a caricamento rapido.
