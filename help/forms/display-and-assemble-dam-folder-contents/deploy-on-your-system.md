---
title: Distribuire le risorse localmente
description: Distribuire le risorse dei tutorial nell’istanza AEM locale
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Implementare sul sistema

Per utilizzare questo caso d’uso nell’istanza AEM locale, segui i passaggi elencati di seguito.

* [Distribuisci il bundle DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) contenuto nel file zip.

* Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** utilizzando [configMgr](http://localhost:4502/system/console/configMgr).

* [Distribuisci il bundle newsletter](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Questo bundle contiene il codice per elencare il contenuto della cartella e assemblare le newsletter selezionate.

* [Importare il pacchetto utilizzando Gestione pacchetti](assets/newsletter.zip). Questo pacchetto contiene la libreria client e file PDF di esempio per testare la soluzione.

* [Importa il modulo adattivo di esempio](assets/sample-adaptive-form.zip). In questo modulo verranno elencate le newsletter che è possibile selezionare.

* [Anteprima modulo](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selezionare un paio di newsletter da scaricare.Le newsletter selezionate verranno combinate in un unico PDF e restituite all&#39;utente.
