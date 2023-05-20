---
title: Acroforms con AEM Forms
description: Tutorial che illustra come creare un modulo adattivo utilizzando Acroform e unire i dati per ottenere un PDF. Il PDF con i dati uniti può quindi essere inviato per la firma utilizzando Acrobat Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---


# Creazione di Forms adattivi da Acroforms

Le organizzazioni dispongono di un&#39;ampia varietà di forme. Alcuni di questi moduli vengono creati in Microsoft Word e convertiti in PDF. Per impostazione predefinita, questi moduli non sono compilabili con Adobe Reader o Acrobat. Per rendere questi moduli compilabili utilizzando Acrobat o Reader, è necessario convertirli in Acroform. Le acroforme sono moduli creati con Acrobat. Questo articolo illustra come creare un modulo adattivo da Acroform e unire nuovamente i dati in Acroform per ottenere il PDF. Il PDF con i dati uniti può essere inviato anche per la firma utilizzando Acrobat Sign.

>[!NOTE]
>
>Se utilizzi AEM Forms 6.5, utilizza la funzionalità di Automated forms conversion.

## Prerequisiti

* AEM Forms 6.3 o 6.4 installato e configurato
* Accesso ad Adobe Acrobat
* Familiarità con AEM/AEM Forms.

### Per utilizzare questa funzionalità nel sistema sono necessari i seguenti elementi

* Scarica e distribuisci i bundle utilizzando [Console web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Scarica e importa questo pacchetto in AEM](assets/acro-form-aem-form.zip). Questo pacchetto contiene il flusso di lavoro di esempio e la pagina HTML per creare XSD da acroform
* Apri [configMgr](http://localhost:4502/system/console/configMgr)
   * Cerca &quot;Apache Sling Service User Mapper Service&quot; e fai clic per aprire le proprietà
   * Fai clic su `+` (più) per aggiungere la seguente mappatura del servizio
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Fai clic su Salva
