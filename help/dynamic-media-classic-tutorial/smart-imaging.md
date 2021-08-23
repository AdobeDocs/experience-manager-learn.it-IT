---
title: Imaging avanzato
description: La funzione Smart imaging in Dynamic Media Classic migliora le prestazioni di distribuzione delle immagini ottimizzando automaticamente il formato e la qualità delle immagini in base alle funzionalità del browser client. A tal fine, sfrutta le funzionalità di Adobe Sensei AI e utilizza i predefiniti per immagini esistenti. Scopri di più sull’imaging avanzato e come utilizzarlo per offrire esperienze migliori ai clienti con carichi di pagina più veloci.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Gestione dei contenuti
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 1%

---


# Imaging avanzato {#smart-imaging}

Uno degli aspetti più importanti dell’esperienza del cliente sul sito web o sul sito mobile o sull’app è il tempo di caricamento della pagina. I clienti abbandonano spesso un sito o un’app se il caricamento di una pagina richiede troppo tempo. Le immagini rappresentano la maggior parte del tempo di caricamento della pagina. La funzione Smart imaging in Dynamic Media Classic migliora le prestazioni di distribuzione delle immagini ottimizzando automaticamente il formato e la qualità delle immagini in base alle funzionalità del browser client. A tal fine, sfrutta le funzionalità di Adobe Sensei AI e utilizza i predefiniti per immagini esistenti. L&#39;imaging intelligente riduce le dimensioni delle immagini del 30% o più, il che si traduce in un caricamento più rapido delle pagine e in una migliore esperienza dei clienti.

L&#39;imaging intelligente trae vantaggio anche dall&#39;ulteriore incremento delle prestazioni, grazie all&#39;integrazione completa con il miglior servizio premium di Adobe. Questo servizio trova il percorso Internet ottimale tra server, reti e punti di peer con latenza minima e/o velocità di perdita dei pacchetti rispetto al percorso predefinito su Internet.

Ulteriori informazioni su [Smart imaging](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html).

## Vantaggi dell&#39;imaging intelligente

Poiché le immagini rappresentano la maggior parte del tempo di caricamento di una pagina, il miglioramento delle prestazioni di Smart Imaging può avere un impatto profondo sui KPI aziendali, ad esempio conversione più elevata, tempo trascorso sul sito e tasso di mancato recapito del sito inferiore.

![immagine](assets/smart-imaging/smart-imaging-1.png)

## Come funziona l’immagine intelligente

Come indicato in precedenza, Smart Imaging sfrutta le funzionalità di Adobe Sensei AI e lavora con i predefiniti per immagini esistenti per convertire automaticamente le immagini in formati di immagine ottimali di nuova generazione, ad esempio WebP, mantenendo al tempo stesso la fedeltà visiva.

Ulteriori informazioni su [Come funziona l&#39;imaging intelligente](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), compresi dettagli quali i formati immagine supportati (e cosa succede se non li usi) e il loro impatto sui predefiniti immagine esistenti in uso.

## Impatto dell&#39;imaging intelligente

È probabile che sarà necessario apportare modifiche agli URL, ai predefiniti per immagini e al codice del sito per sfruttare lo Smart imaging. Se si soddisfano i prerequisiti per l&#39;utilizzo di Smart Imaging e si lavora solo con le immagini nei formati immagine JPEG e PNG supportati, non è necessario apportare alcuna modifica.

L’imaging avanzato funziona con le immagini distribuite tramite HTTP, HTTPS e HTTP/2.

>[!NOTE]
>
>Il passaggio a Smart imaging cancella la cache alla rete CDN. La cache nel CDN viene generalmente ricostruita di nuovo entro uno o due giorni.

La funzione Smart imaging è inclusa nella licenza esistente di Dynamic Media Classic. Questa funzione non comporta costi aggiuntivi. Per sfruttarlo, devi soddisfare due requisiti: dispongono di una rete CDN con bundle di Adobe e di un dominio dedicato. Quindi devi abilitarlo per il tuo account perché non è abilitato automaticamente.

L’abilitazione di Smart imaging inizia con l’invio del supporto tecnico a una richiesta di |creazione di un caso di supporto| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Il supporto consente di impostare un dominio personalizzato da associare a Smart imaging. Modificherai un parametro relativo alla memorizzazione in cache (Time To Live, o TTL) e il supporto cancellerà la cache. Puoi anche effettuare un passaggio di staging facoltativo se lo desideri prima di passare alla produzione. Poi, quando Smart imaging è attivato, i clienti potranno distribuire immagini di dimensioni più piccole, ma con la stessa qualità richiesta. Ciò significa che sperimentano tempi di caricamento delle pagine più rapidi e tutto questo viene fatto automaticamente perché Adobe Sensei aiuta a scegliere la dimensione più efficiente.

Dopo aver abilitato Smart imaging, dovrai verificare che funzioni come previsto.

Probabilmente hai ulteriori domande su Smart imaging. Abbiamo compilato un elenco delle domande frequenti (FAQ) con le risposte. Leggi le [Domande frequenti](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html).

## Risorse aggiuntive

Guarda il webinar on-demand [Dynamic Media Classic Optimizing Page Performance Generatore](https://seminars.adobeconnect.com/pzc1gw0cihpv) per ulteriori informazioni sull&#39;imaging intelligente.
