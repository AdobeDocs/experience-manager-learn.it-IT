---
title: Acroformi con AEM Forms
description: Esercitazione che illustra come creare un modulo adattivo utilizzando Acroform e come unire i dati per ottenere un PDF. È quindi possibile inviare il PDF con i dati uniti per la firma utilizzando Adobe Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---


# Creazione di Forms adattivo da Acroforms

Le organizzazioni hanno un&#39;ampia varietà di forme. Alcuni di questi moduli vengono creati in Microsoft Word e convertiti in PDF. Per impostazione predefinita, questi moduli non sono compilabili utilizzando Adobe Reader o Acrobat. Per rendere questi moduli compilabili utilizzando Acrobat o Reader, è necessario convertirli in Acroform. I moduli sono creati con Acrobat. Questo articolo illustra come creare un modulo adattivo da Acroform e come riunire i dati in Acroform per ottenere il PDF. È inoltre possibile inviare il PDF con i dati uniti per la firma tramite Adobe Sign.

>[!NOTE]
>
>Se utilizzi AEM Forms 6.5, utilizza la funzionalità Automated forms conversion.

## Prerequisiti

* AEM Forms 6.3 o 6.4 installato e configurato
* Accesso ad Adobe Acrobat
* Familiarità con AEM/AEM Forms.

### Per far funzionare questa funzionalità sul sistema, è necessario quanto segue

* Scarica e distribuisci i bundle utilizzando [Console Web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Scarica e importa questo pacchetto in AEM](assets/acro-form-aem-form.zip). Questo pacchetto contiene il flusso di lavoro di esempio e la pagina HTML per creare XSD da acroform
* Apri [configMgr](http://localhost:4502/system/console/configMgr)
   * Cerca &#39;Apache Sling Service User Mapper Service&#39; e fai clic per aprire le proprietà
   * Fai clic sul pulsante `+` icona (più) per aggiungere la seguente mappatura del servizio
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Fai clic su &#39;Salva&#39;
