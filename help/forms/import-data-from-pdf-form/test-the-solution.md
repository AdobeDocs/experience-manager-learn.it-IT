---
title: Importare dati da un file PDF in un modulo adattivo
description: Esercitazione per compilare un modulo adattivo importando un file PDF
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Distribuire le risorse di esempio

Puoi distribuire le risorse di esempio per far funzionare questa soluzione nell’istanza AEM Forms locale

* [Importa la libreria client e il componente personalizzato per caricare il modulo pdf tramite Gestione pacchetti](./assets/client-libs-custom-component.zip)
* Scarica e distribuisci il bundle utilizzando la console Web OSGi[Bundle servizi documentali personalizzati](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Scarica e distribuisci il bundle utilizzando la console Web OSGi [Sviluppo con Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Scarica e distribuisci il bundle utilizzando la console Web OSGi[importa dati dal file pdf](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Aggiungi la voce _&#x200B;**DevelopingWithServiceUser.core:getresourceresolver=data**&#x200B;_ nella console di configurazione OSGi _&#x200B;**Apache Sling Service User Mapper Service**&#x200B;_
