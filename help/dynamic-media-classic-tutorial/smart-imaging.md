---
title: Imaging avanzato
description: La tecnologia Smart Imaging di Dynamic Media Classic migliora le prestazioni di consegna delle immagini ottimizzando automaticamente il formato e la qualità delle immagini in base alle funzionalità del browser client. A tal fine, sfrutta le funzionalità di intelligenza artificiale di Adobe Sensei e utilizza i predefiniti immagine esistenti. Scopri di più sulla tecnologia Smart Imaging e come utilizzarla per offrire ai clienti esperienze migliori attraverso caricamenti di pagina più veloci.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 1%

---

# Imaging avanzato {#smart-imaging}

Uno degli aspetti più importanti dell’esperienza del cliente sul sito web o sul sito mobile o sull’app è il tempo di caricamento della pagina. Spesso i clienti abbandonano un sito o un’app se il caricamento di una pagina richiede troppo tempo. Le immagini costituiscono la maggior parte del tempo di caricamento della pagina. La tecnologia Smart Imaging di Dynamic Media Classic migliora le prestazioni di consegna delle immagini ottimizzando automaticamente il formato e la qualità delle immagini in base alle funzionalità del browser client. A tal fine, sfrutta le funzionalità di intelligenza artificiale di Adobe Sensei e utilizza i predefiniti immagine esistenti. La tecnologia Smart Imaging riduce le dimensioni delle immagini del 30% o più, velocizzando il caricamento delle pagine e migliorando le esperienze dei clienti.

L’imaging intelligente beneficia inoltre dell’aumento delle prestazioni derivante dalla piena integrazione con il servizio Adobe di prima qualità. Questo servizio individua il percorso Internet ottimale tra server, reti e peer point con latenza e/o frequenza di perdita dei pacchetti più bassa rispetto al percorso predefinito su Internet.

Ulteriori informazioni su [Smart Imaging](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=it).

## Vantaggi dell&#39;imaging intelligente

Poiché le immagini rappresentano la maggior parte del tempo di caricamento di una pagina, il miglioramento delle prestazioni ottenuto con Smart Imaging può avere un impatto profondo sui KPI aziendali, ad esempio una conversione più elevata, il tempo trascorso sul sito e una percentuale inferiore di mancato recapito del sito.

![immagine](assets/smart-imaging/smart-imaging-1.png)

## Funzionamento dell&#39;imaging avanzato

Come accennato in precedenza, Smart Imaging sfrutta le funzionalità di intelligenza artificiale di Adobe Sensei e funziona con i predefiniti immagine esistenti per convertire automaticamente le immagini in formati di immagine di nuova generazione ottimali, come WebP, mantenendo al contempo la fedeltà visiva.

Ulteriori informazioni su [Funzionamento di Smart Imaging](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=it#how-does-smart-imaging-work), inclusi dettagli quali i formati immagine supportati (e cosa accade se non si utilizzano tali formati) e il relativo impatto sui predefiniti immagine esistenti in uso.

## Impatto dell&#39;imaging intelligente

Probabilmente dovrai apportare modifiche agli URL, ai predefiniti immagine e al codice sul tuo sito per sfruttare i vantaggi di Smart Imaging. Se si soddisfano i prerequisiti per utilizzare Smart Imaging e si utilizzano solo immagini nei formati JPEG e PNG supportati, non è necessario apportare alcuna modifica.

L’imaging avanzato funziona con le immagini distribuite tramite HTTP, HTTPS e HTTP/2.

>[!NOTE]
>
>Il passaggio a Smart Imaging cancella la cache dalla rete CDN. In genere, la cache nella rete CDN viene creata nuovamente entro uno o due giorni.

Smart Imaging è incluso nella licenza Dynamic Media Classic esistente. Questa funzione non comporta costi aggiuntivi. Per sfruttarla, devi soddisfare due requisiti: disporre di una rete CDN inclusa nel pacchetto di Adobi e di un dominio dedicato. Quindi devi abilitarlo per il tuo account perché non è abilitato automaticamente.

L’abilitazione di Smart Imaging inizia con l’invio di una richiesta al supporto tecnico da parte di |creazione di un caso di supporto| [https://helpx.adobe.com/it/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/it/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Il supporto ti aiuterà a configurare un dominio personalizzato da associare a Smart Imaging. Modificherai un parametro relativo al caching (Time To Live, o TTL) e il supporto cancellerà la cache. Puoi anche eseguire un passaggio di staging facoltativo, se lo desideri, prima di eseguire il push in produzione. Quindi, quando la tecnologia Smart Imaging è attivata, si forniranno ai clienti immagini di dimensioni più piccole, ma con la stessa qualità richiesta. Ciò significa che sperimentano tempi di caricamento delle pagine più veloci e tutto questo viene fatto automaticamente perché Adobe Sensei aiuta a scegliere le dimensioni più efficienti.

Dopo aver attivato Smart Imaging, è necessario verificare che funzioni come previsto.

Probabilmente hai ulteriori domande sull’imaging avanzato. Abbiamo compilato un elenco di domande frequenti (FAQ) con le risposte. Leggi le [domande frequenti](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=it).

## Risorse aggiuntive

Guarda il webinar on-demand [Dynamic Media Classic Optimizing Page Performance Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) per ulteriori informazioni sull&#39;imaging avanzato.
