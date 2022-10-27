---
title: Creare HTML5 Forms
description: Creazione e configurazione di moduli HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 4%

---

# Creazione di moduli HTML5

HTML5 forms è una nuova funzionalità di Adobe Experience Manager che offre il rendering di modelli di moduli XFA (xdp) in formato HTML5. Questa funzionalità consente di effettuare il rendering dei moduli su dispositivi mobili e browser desktop che non supportano i PDF basati su XFA. HTML5 forms supporta non solo le funzionalità esistenti dei modelli di modulo XFA, ma aggiunge anche nuove funzionalità per i dispositivi mobili, ad esempio la firma a mano libera.

## Prerequisito

Assicurati di disporre di un&#39;istanza funzionante di AEM Forms. Segui la [guida all&#39;installazione](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) per installare e configurare AEM Forms

## Creare il primo modulo HTML5

1. [Scaricare ed estrarre il contenuto del file zip](assets/assets.zip). Il file zip contiene file xdp e di dati
2. [Passa a Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Fai clic su Crea -> Caricamento file
4. Seleziona il modello xdp scaricato al passaggio 2

## Anteprima come HTML

È possibile visualizzare in anteprima l’xdp in formato HTML5 o PDF. Per visualizzare in anteprima l&#39;xdp in formato HTML5, segui i seguenti passaggi

* Tocca l’xdp appena caricato e fai clic su _Anteprima -> Anteprima come HTML_. Dovresti visualizzare l’xdp renderizzato come HTML5

>[!NOTE]
>Quando selezioni _Anteprima come PDF_ l&#39;opzione renderizzata PDF non sarà visualizzata nel browser perché AEM Forms esegue il rendering di pdf dinamici che richiedono il plugin Acrobat.Sarà necessario scaricare il PDF e aprirlo utilizzando Adobe Acrobat/Reader per visualizzare


## Anteprima con i dati

Per visualizzare in anteprima l&#39;xdp in formato HTML5 con il file di dati, segui i seguenti passaggi:

* Tocca l’xdp appena caricato e fai clic su _Anteprima -> Anteprima con dati_. Sfoglia e seleziona il file di dati e fai clic su _Anteprima_.
* Dovresti vedere il modello di cui è stato eseguito il rendering in formato HTML5 precompilato con i dati

## Esplora proprietà avanzate del modello xdp

Le proprietà avanzate del modello xdp consentono di specificare la data di pubblicazione, il gestore di invio, il profilo di rendering del modulo, il servizio di precompilazione, ecc. Per visualizzare le proprietà avanzate del modello, tocca sull’xdp e fai clic su _proprietà -> Avanzate_. Qui troverete una serie di proprietà. Alcune di queste proprietà sono coperte qui.

**Invia URL** - Questo è l’URL che gestirà l’invio del modulo HTML5. Ne parleremo nella prossima lezione. Se non si specifica un URL di invio qui, viene richiamato il gestore di invio predefinito che restituisce i dati del modulo al browser.

**Profilo di rendering HTML** - I moduli di HTML5 hanno la nozione di profili esposti come endpoint REST per abilitare il rendering mobile dei modelli di modulo. La maggior parte delle volte il profilo di rendering predefinito deve essere sufficiente per eseguire il rendering del modulo. Se il profilo di rendering predefinito non soddisfa le tue esigenze, un [profilo personalizzato](https://experienceleague.adobe.com/docs/experience-manager-64/forms/html5-forms/custom-profile.html) può essere creato e associato al modulo.

**Servizio di precompilazione** - Il servizio di precompilazione viene in genere utilizzato per compilare il modulo con i dati recuperati da un’origine dati back-end.
