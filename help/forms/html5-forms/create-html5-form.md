---
title: Crea Forms HTML5
description: Creazione e configurazione di moduli HTML5
feature: Forms Mobile
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Sviluppo
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 5%

---


# Creazione di moduli HTML5

I moduli HTML5 sono una nuova funzionalità di Adobe Experience Manager che offre il rendering di modelli di moduli XFA (xdp) in formato HTML5. Questa funzionalità consente di effettuare il rendering dei moduli su dispositivi mobili e browser desktop che non supportano i PDF basati su XFA. I moduli HTML5 non solo supportano le funzionalità esistenti dei modelli di moduli XFA, ma aggiungono anche nuove funzionalità per i dispositivi mobili, come la firma a mano libera.

## Prerequisito

Assicurati di disporre di un&#39;istanza funzionante di AEM Forms. Segui la [guida all&#39;installazione](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) per installare e configurare AEM Forms

## Crea il primo modulo HTML5

1. [Scarica ed estrae il contenuto del file](assets/assets.zip) zip. Il file zip contiene file xdp e di dati
2. [Passa a Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Fai clic su Crea -> Caricamento file
4. Seleziona il modello xdp scaricato al passaggio 2

## Anteprima come HTML

L&#39;xdp può essere visualizzato in anteprima in formato HTML5 o PDF. Per visualizzare in anteprima l&#39;xdp in formato HTML5, segui i seguenti passaggi

* Tocca l’xdp appena caricato e fai clic su _Anteprima -> Anteprima come HTML_. Dovresti vedere l&#39;xdp renderizzato come HTML5

>[!NOTE]
>Quando si seleziona l&#39;opzione _Anteprima come PDF_, il PDF di cui è stato effettuato il rendering non verrà visualizzato nel browser, perché AEM Forms esegue il rendering dei pdf dinamici che richiedono il plug-in Acrobat.Per visualizzare il PDF, è necessario scaricare il PDF e aprirlo con Adobe Acrobat/Reader.


## Anteprima con i dati

Per visualizzare in anteprima l&#39;xdp in formato HTML5 con file di dati, segui i seguenti passaggi:

* Tocca l’xdp appena caricato e fai clic su _Anteprima -> Anteprima con dati_. Sfoglia e seleziona il file di dati e fai clic su _Anteprima_.
* Dovresti vedere il modello renderizzato in formato HTML5 precompilato con i dati

## Esplora proprietà avanzate del modello xdp

Le proprietà avanzate del modello xdp consentono di specificare la data di pubblicazione, il gestore di invio, il profilo di rendering del modulo, il servizio di precompilazione, ecc. Per visualizzare le proprietà avanzate del modello, tocca sull’xdp e fai clic su _proprietà -> Avanzate_. Qui troverete una serie di proprietà. Alcune di queste proprietà sono coperte qui.

**URL di invio** : questo è l’URL che gestirà l’invio del modulo HTML5. Ne parleremo nella prossima lezione. Se non si specifica un URL di invio qui, viene richiamato il gestore di invio predefinito che restituisce i dati del modulo al browser.

**Profilo di rendering HTML** : i moduli HTML5 hanno la nozione di profili esposti come endpoint REST per abilitare il rendering mobile dei modelli di modulo. La maggior parte delle volte il profilo di rendering predefinito deve essere sufficiente per eseguire il rendering del modulo. Se il profilo di rendering predefinito non soddisfa le tue esigenze, è possibile creare e associare al modulo un [profilo personalizzato](https://experienceleague.adobe.com/docs/experience-manager-64/forms/html5-forms/custom-profile.html).

**Servizio di precompilazione** : il servizio di precompilazione viene in genere utilizzato per compilare il modulo con i dati recuperati da un’origine dati back-end.

