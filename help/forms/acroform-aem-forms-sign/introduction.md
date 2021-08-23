---
title: Acroformi con AEM Forms
description: Esercitazione che illustra come creare un modulo adattivo utilizzando Acrobat e come unire i dati per ottenere un PDF. Il PDF con i dati uniti può quindi essere inviato per la firma utilizzando Adobe Sign.
feature: moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

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

* Scarica e distribuisci i bundle utilizzando la [Console Web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Scarica e importa questo pacchetto in AEM](assets/acro-form-aem-form.zip). Questo pacchetto contiene il flusso di lavoro di esempio e la pagina HTML per creare XSD da acroform
* Apri [configMgr](http://localhost:4502/system/console/configMgr)
   * Cerca &#39;Apache Sling Service User Mapper Service&#39; e fai clic per aprire le proprietà
   * Fai clic sull&#39;icona `+` (più) per aggiungere la seguente Mappatura del servizio
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Fai clic su &#39;Salva&#39;
