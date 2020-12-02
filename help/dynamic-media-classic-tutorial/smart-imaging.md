---
title: Imaging avanzato
description: La funzione Smart Imaging in Dynamic Media Classic migliora le prestazioni di distribuzione delle immagini ottimizzando automaticamente il formato e la qualità delle immagini in base alle funzionalità del browser client. A questo scopo, utilizza  funzionalità di Adobe Sensei AI e utilizza i predefiniti per immagini esistenti. Scopri ulteriori informazioni su Smart Imaging e su come utilizzarlo per offrire ai clienti esperienze migliori attraverso un caricamento più rapido delle pagine.
sub-product: dynamic-media
feature: smart-crop
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 317fb625e7af57b7ad0079014c341eab9adda376
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 1%

---


# Imaging avanzato {#smart-imaging}

Uno degli aspetti più importanti dell&#39;esperienza cliente sul sito Web o sul sito mobile o sull&#39;app è il tempo di caricamento della pagina. I clienti spesso abbandonano un sito o un&#39;app se il caricamento di una pagina richiede troppo tempo. Le immagini rappresentano la maggior parte del tempo di caricamento della pagina. La funzione Smart Imaging in Dynamic Media Classic migliora le prestazioni di distribuzione delle immagini ottimizzando automaticamente il formato e la qualità delle immagini in base alle funzionalità del browser client. A questo scopo, utilizza  funzionalità di Adobe Sensei AI e utilizza i predefiniti per immagini esistenti. L&#39;imaging intelligente riduce le dimensioni delle immagini del 30% o più — che si traduce in un caricamento delle pagine più veloce ed esperienze cliente migliori.

La funzione Smart Imaging migliora inoltre le prestazioni grazie all&#39;integrazione completa con il servizio premium di livello superiore  Adobe. Questo servizio trova la via Internet ottimale tra server, reti e punti di peering con latenza minima e/o tasso di perdita dei pacchetti rispetto alla route predefinita su Internet.

Ulteriori informazioni su [Smart Imaging](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Vantaggi delle immagini intelligenti

Poiché le immagini costituiscono la maggior parte del tempo di caricamento di una pagina, il miglioramento delle prestazioni di Smart Imaging può avere un impatto profondo sui KPI aziendali, come conversione più elevata, tempo trascorso sul sito e tasso di rimbalzo inferiore del sito.

![immagine](assets/smart-imaging/smart-imaging-1.png)

## Come funziona l&#39;immagine intelligente

Come già detto, Smart Imaging sfrutta  funzionalità di Adobe Sensei AI e utilizza i predefiniti per immagini esistenti per convertire automaticamente le immagini in formati di immagine di nuova generazione ottimali, come il WebP, mantenendo al contempo la fedeltà visiva.

Ulteriori informazioni su [Come funziona l&#39;imaging intelligente](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), inclusi dettagli quali i formati immagine supportati (e cosa accade se non si utilizzano tali formati) e il relativo impatto sui predefiniti per immagini esistenti in uso.

## Impatto delle immagini intelligenti

È probabile che dobbiate apportare modifiche agli URL, ai predefiniti per immagini e al codice presenti sul sito per sfruttare la funzione Smart Imaging. Se si soddisfano i prerequisiti per l’utilizzo di Smart Imaging e si utilizzano solo immagini nei formati di immagine JPEG e PNG supportati, non è necessario apportare alcuna modifica.

La funzione di imaging avanzato funziona con immagini distribuite tramite HTTP, HTTPS e HTTP/2.

>[!NOTE]
>
>Il passaggio a Smart Imaging cancella la cache sulla rete CDN. La cache della CDN viene in genere ricreata entro uno o due giorni.

La funzione Smart Imaging è inclusa nella licenza esistente di Dynamic Media Classic. Non sono previsti costi aggiuntivi per questa funzione. Per sfruttarlo, è necessario soddisfare due requisiti: dispongono di un CDN  Adobe e di un dominio dedicato. A questo punto, è necessario attivarlo per il proprio account perché non è attivato automaticamente.

L&#39;abilitazione di Smart Imaging inizia con l&#39;invio di un&#39;assistenza tecnica tramite |creazione di un caso di supporto| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Il supporto vi consentirà di impostare un dominio personalizzato che verrà associato a Smart Imaging. Modificherete un parametro relativo alla memorizzazione nella cache (Time To Live, o TTL) e il supporto cancellerà la cache. È inoltre possibile eseguire un passaggio di staging facoltativo se lo si desidera prima di passare alla produzione. Quando la funzione Smart Imaging è attivata, i clienti potranno ottenere immagini di dimensioni ridotte, ma con la stessa qualità richiesta. Ciò significa che i tempi di caricamento delle pagine sono più rapidi, e tutto questo viene fatto automaticamente perché  Adobe Sensei aiuta a scegliere le dimensioni più efficienti.

Dopo aver attivato Smart Imaging, è necessario verificare che funzioni come previsto.

Probabilmente avete ulteriori domande su Smart Imaging. Abbiamo compilato un elenco delle domande frequenti con le risposte. Leggere le [FAQ](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Risorse aggiuntive

Guardate il webinar on-demand [Dynamic Media Classic Optimizing Page Performance Performance Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) per saperne di più sull&#39;imaging intelligente.
