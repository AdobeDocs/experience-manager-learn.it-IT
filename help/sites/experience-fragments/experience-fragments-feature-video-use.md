---
title: Utilizzo AEM frammenti esperienza
description: I frammenti esperienza consentono agli autori dei contenuti di riutilizzare i contenuti tra canali, incluse le pagine Siti e i sistemi di terze parti.
sub-product: siti, servizi di contenuto
feature: experience-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 971
translation-type: tm+mt
source-git-commit: 3b5dd583a458393a41dbce1d8eeb0095a22db734
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Utilizzo di Frammenti esperienza{#using-aem-experience-fragments}

I frammenti esperienza sono una funzione di Adobe Experience Manager (AEM), introdotta per la prima volta in AEM 6.3. I frammenti esperienza consentono agli autori dei contenuti di riutilizzare i contenuti tra canali, incluse le pagine Siti e i sistemi di terze parti.

## Panoramica sui frammenti esperienza

>[!VIDEO](https://video.tv.adobe.com/v/17028/?quality=9&learn=on)

Un frammento esperienza è un insieme di contenuti che raggruppano un&#39;esperienza che dovrebbe avere senso da sola.

Con i frammenti esperienza, gli addetti marketing possono:

* Riutilizzare un&#39;esperienza tra canali (canali di proprietà e punti di contatto di terze parti)
* Creare varianti di un&#39;esperienza per casi di utilizzo specifici
* Mantenere sincronizzate le varianti con l&#39;utilizzo di Live Copy
* Esperienze di Social Post su Facebook e Pinterest pronte all&#39;uso

## Creazione di blocchi con frammenti esperienza

I blocchi predefiniti sono un nuovo miglioramento aggiunto al frammento esperienza in AEM 6.4+. Consente agli autori dei contenuti di creare un blocco predefinito composto da componenti che possono essere riutilizzati per creare contenuti tra diverse varianti e tra diversi modelli.

>[!VIDEO](https://video.tv.adobe.com/v/21289/?quality=9&learn=on)

>[!NOTE]
>
> I modelli modificabili utilizzati per creare frammenti esperienza devono includere il componente blocco predefinito nei relativi criteri.

* La creazione di un blocco predefinito facilita il riutilizzo dei contenuti da parte degli autori di contenuti per diverse varianti.
* Se si modifica il blocco predefinito per la copia master, le modifiche apportate ai riferimenti verranno automaticamente implementate senza annullare l&#39;ereditarietà o apportare modifiche al layout.
* Gli autori dei contenuti possono rinominare o eliminare facilmente un blocco predefinito esistente.
* Se si elimina un blocco predefinito da un frammento esperienza, i riferimenti non vengono eliminati.

## Ricerca full-text in frammenti esperienza

AEM 6.5 ora supporta funzionalità di ricerca full-text per i frammenti esperienza.

>[!VIDEO](https://video.tv.adobe.com/v/27720/?quality=9&learn=on)

* **Gli autori** dei contenuti (ricerca interna) ora possono cercare una parte di testo all&#39;interno di un frammento esperienza e il risultato includerebbe i frammenti esperienza contenenti il testo e la pagina che fa riferimento al frammento esperienza.
* **Gli utenti** del sito (ricerca esterna) possono ora eseguire una ricerca full-text utilizzando il componente di ricerca e il risultato include pagine del sito che fanno riferimento al frammento esperienza contenente la parola chiave search.
