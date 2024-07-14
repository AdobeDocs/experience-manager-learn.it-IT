---
title: Acroforms con AEM Forms
description: Tutorial che illustra come creare un modulo adattivo utilizzando Acroform e unire i dati per ottenere un PDF. Il PDF con i dati uniti può quindi essere inviato per la firma utilizzando Acrobat Sign.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

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

* Scarica e distribuisci i bundle utilizzando la [console Web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Scaricare e importare il pacchetto in AEM](assets/acro-form-aem-form.zip). Questo pacchetto contiene il flusso di lavoro di esempio e la pagina HTML per creare XSD da acroform
* Apri [configMgr](http://localhost:4502/system/console/configMgr)
   * Cerca &quot;Apache Sling Service User Mapper Service&quot; e fai clic per aprire le proprietà
   * Fai clic sull&#39;icona `+` (segno più) per aggiungere il seguente mapping dei servizi
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Fai clic su Salva
