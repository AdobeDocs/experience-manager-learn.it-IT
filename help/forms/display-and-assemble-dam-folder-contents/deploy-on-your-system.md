---
title: Distribuzione locale delle risorse
description: Distribuire le risorse dell’esercitazione nell’istanza AEM locale
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 5%

---

# Distribuzione nel sistema

Segui i passaggi elencati di seguito per far funzionare questo caso d’uso sull’istanza AEM locale.

* [Distribuire il bundle DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) contenuto nel file zip.

* Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** utilizzando [configMgr](http://localhost:4502/system/console/configMgr).

* [Distribuzione del bundle newsletter](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Questo bundle contiene il codice per elencare il contenuto della cartella e assemblare le newsletter selezionate.

* [Importa il pacchetto utilizzando Gestione pacchetti](assets/newsletter.zip). Questo pacchetto contiene la libreria client e file pdf di esempio per testare la soluzione.

* [Importare il modulo adattivo di esempio](assets/sample-adaptive-form.zip). In questo modulo vengono elencate le newsletter che è possibile selezionare.

* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selezionare un paio di newsletter da scaricare.Le newsletter selezionate verranno combinate in un pdf e restituite all&#39;utente.




