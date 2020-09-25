---
title: Creare HTML5 Forms
description: Creazione e configurazione di moduli HTML5
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 4%

---


# Creare moduli HTML5

I moduli HTML5 sono una nuova funzionalità di Adobe Experience Manager che offre il rendering di modelli di modulo XFA (xdp) in formato HTML5. Questa funzionalità consente di effettuare il rendering dei moduli su dispositivi mobili e browser desktop che non supportano i PDF basati su XFA. I moduli HTML5 non solo supportano le funzionalità esistenti dei modelli di modulo XFA, ma aggiungono anche nuove funzionalità per i dispositivi mobili, come la firma a mano.

## Prerequisito

Verificare di disporre di un&#39;istanza funzionante di  AEM Forms. Seguite la guida [all&#39;](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) installazione per installare e configurare  AEM Forms

## Creare il primo modulo HTML5

1. [Scaricate ed estraete il contenuto del file](assets/assets.zip)zip. Il file zip contiene xdp e il file di dati
2. [Passa ad Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Fate clic su Crea > Caricamento file
4. Selezionate il modello xdp scaricato al punto 2

## Anteprima come HTML

L’xdp può essere visualizzato in anteprima in formato HTML5 o PDF. Per visualizzare l&#39;anteprima dell&#39;xdp in formato HTML5, effettuate le seguenti operazioni

* Toccate il file xdp appena caricato e fate clic su _Anteprima > Anteprima come HTML_. Dovreste visualizzare l’xdp rappresentato in HTML5

>[!NOTE]
>Quando si seleziona l&#39;opzione _Anteprima come PDF_ , il PDF di cui è stato effettuato il rendering non verrà visualizzato nel browser perché  AEM Forms esegue il rendering di PDF dinamici che richiedono  plug-in Acrobat.Sarà necessario scaricare il PDF e aprirlo  Adobe Acrobat/Reader per visualizzare


## Anteprima con i dati

Per visualizzare l&#39;anteprima dell&#39;xdp in formato HTML5 con il file di dati, attenetevi alla seguente procedura:

* Toccate il file xdp appena caricato e fate clic su _Anteprima > Anteprima con dati_. Individuate e selezionate il file di dati e fate clic su _Anteprima_.
* Dovresti vedere il modello rappresentato in formato HTML5 precompilato con i dati

## Esplora proprietà avanzate del modello xdp

Le proprietà avanzate del modello xdp consentono di specificare la data di pubblicazione, il gestore di invio, il profilo di rendering del modulo, il servizio di precompilazione ecc. Per visualizzare le proprietà avanzate del modello, toccate il file xdp e fate clic su _Proprietà -> Avanzate_. Qui troverete una serie di proprietà. Alcune di queste proprietà sono coperte qui.

**Invia URL** - Questo è l&#39;URL che gestirà l&#39;invio del modulo HTML5. Lo affronteremo nella prossima lezione. Se non si specifica alcun URL di invio, viene richiamato il gestore di invio predefinito che restituisce i dati del modulo al browser.

**Profilo** di rendering HTML - I moduli HTML5 hanno la nozione di profili esposti come endpoint REST per abilitare il rendering mobile dei modelli di modulo. La maggior parte delle volte il profilo di rendering predefinito deve essere sufficiente per eseguire il rendering del modulo. Se il profilo di rendering predefinito non soddisfa le esigenze, è possibile creare e associare al modulo un profilo [](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) personalizzato.

**Servizio** di precompilazione: il servizio di precompilazione viene in genere utilizzato per compilare il modulo con i dati recuperati da un&#39;origine dati back-end.

