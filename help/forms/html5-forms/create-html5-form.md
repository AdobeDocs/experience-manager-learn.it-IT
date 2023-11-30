---
title: Crea Forms HTML5
description: Creare e configurare moduli di HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# Creare moduli HTML5

HTML5 forms è una nuova funzionalità di Adobe Experience Manager che offre il rendering dei modelli di modulo XFA (xdp) in formato HTML5. Questa funzionalità consente di eseguire il rendering dei moduli su dispositivi mobili e browser desktop su cui non è supportato il PDF basato su XFA. HTML5 Forms non solo supporta le funzionalità esistenti dei modelli di modulo XFA, ma aggiunge anche nuove funzionalità, come la firma scarabocchio, per i dispositivi mobili.

## Prerequisito

Assicurati di disporre di un’istanza funzionante di AEM Forms. Segui le istruzioni [guida all’installazione](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) per installare e configurare AEM Forms

## Creare il primo modulo HTML5

1. [Scarica ed estrai il contenuto del file zip](assets/assets.zip). Il file zip contiene xdp e file di dati
2. [Passa a Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Fai clic su Crea -> Caricamento file
4. Seleziona il modello xdp scaricato nel passaggio 2

## Anteprima come HTML

L’xdp può essere visualizzato in anteprima in formato HTML5 o PDF. Per visualizzare l’anteprima del file xdp in formato HTML5, segui i passaggi seguenti

* Tocca l’XDP appena caricato e fai clic su _Anteprima -> Anteprima come HTML_. Dovresti visualizzare l’XDP renderizzato come HTML5

>[!NOTE]
>Quando selezioni _Anteprima come PDF_ opzione il PDF renderizzato non verrà visualizzato nel browser perché AEM Forms esegue il rendering dei pdf dinamici che richiedono il plug-in di Acrobat. Per visualizzare il PDF dovrai scaricare il rendering e aprirlo utilizzando Adobe Acrobat/Reader


## Anteprima con i dati

Per visualizzare l’anteprima del file xdp in formato HTML5 con file di dati, effettua le seguenti operazioni:

* Tocca l’XDP appena caricato e fai clic su _Anteprima -> Anteprima con dati_. Sfoglia e seleziona il file di dati e fai clic su _Anteprima_.
* Dovresti vedere il modello renderizzato in formato HTML5, precompilato con i dati

## Esplora le proprietà avanzate del modello XDP

Le proprietà avanzate del modello xdp ti consentono di specificare la data di pubblicazione, il gestore di invio, il profilo di rendering per il modulo, il servizio di precompilazione, ecc. Per visualizzare le proprietà avanzate del modello, tocca il file xdp e fai clic su _properties -> Advanced_. Qui troverai una serie di proprietà. Alcune di queste proprietà sono coperte qui.

**Invia URL** : questo è l’URL che gestirà l’invio del modulo HTML5. Questo argomento verrà trattato nella prossima lezione. Se non viene specificato un URL di invio, viene richiamato il gestore di invio predefinito che restituisce i dati del modulo al browser.

**Profilo rendering HTML** - I moduli HTML5 hanno il concetto di profili esposti come endpoint REST per abilitare il rendering mobile dei modelli di modulo. La maggior parte delle volte il profilo di rendering predefinito deve essere sufficiente per eseguire il rendering del modulo. Se il profilo di rendering predefinito non soddisfa le tue esigenze, [profilo personalizzato](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) possono essere create e associate al modulo.

**Servizio preriempimento** : il servizio di precompilazione viene in genere utilizzato per compilare il modulo con dati recuperati da un’origine dati back-end.
