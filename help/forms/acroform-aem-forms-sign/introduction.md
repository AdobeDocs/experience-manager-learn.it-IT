---
title: Acrobat con  AEM Forms
description: Esercitazione che illustra come creare un modulo adattivo utilizzando Acrobat e unire i dati per ottenere un PDF. Il PDF con i dati uniti può quindi essere inviato per la firma tramite  Adobe Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Creazione di Forms adattivo da Acrobat

Le organizzazioni hanno un&#39;ampia varietà di moduli. Alcuni di questi moduli vengono creati in Microsoft Word e convertiti in PDF. Per impostazione predefinita, questi moduli non possono essere compilati utilizzando  Adobe Reader o  Acrobat. Per rendere i moduli compilabili utilizzando  Acrobat o Reader, è necessario convertire questi moduli in Acrobat. I moduli Acrobat sono moduli creati utilizzando  Acrobat. In questo articolo viene descritto come creare un modulo adattivo da Acrobat e unire di nuovo i dati in Acrobat per ottenere il PDF. È inoltre possibile inviare il PDF con i dati uniti per la firma tramite  Adobe Sign.

>[!NOTE]
>
>Se utilizzate  AEM Forms 6.5, utilizzate la funzionalità Automated forms conversion.

## Prerequisiti

*  AEM Forms 6.3 o 6.4 installato e configurato
* Accesso a  Adobe Acrobat
* Familiarità con AEM Forms AEM/.

### Per utilizzare questa funzionalità sul sistema, sono necessari i seguenti requisiti:

* Scaricate e distribuite i bundle utilizzando la [Console Web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Scaricate e importate il pacchetto in AEM](assets/acro-form-aem-form.zip). Questo pacchetto contiene il flusso di lavoro di esempio e la pagina html per creare XSD da acroform
* Aprire la [configMgr](http://localhost:4502/system/console/configMgr)
   * Cercare &#39;Apache Sling Service User Mapper Service&#39; e fare clic per aprire le proprietà
   * Fate clic sull&#39;icona `+` (più) per aggiungere la seguente mappatura dei servizi
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Fare clic su &#39;Save&#39;
