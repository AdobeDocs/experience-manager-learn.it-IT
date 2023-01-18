---
title: Distribuzione locale delle risorse
description: Distribuire le risorse dell’esercitazione nell’istanza AEM locale
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 2%

---

# Distribuzione nel sistema

Segui i passaggi elencati di seguito per far funzionare questo caso d’uso sull’istanza AEM locale.

* [Configura l&#39;utilizzo dell&#39;utente del servizio fd seguendo i passaggi indicati in questo articolo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). Assicurati di aver implementato il bundle DevelopingWithServiceUser .

* [Distribuzione del bundle newsletter](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Questo bundle contiene il codice per elencare il contenuto della cartella e assemblare le newsletter selezionate.

* [Importa il pacchetto utilizzando Gestione pacchetti](assets/newsletter.zip). Questo pacchetto contiene la libreria client e file pdf di esempio per testare la soluzione.

* [Importare il modulo adattivo di esempio](assets/sample-adaptive-form.zip). In questo modulo vengono elencate le newsletter che è possibile selezionare.

* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selezionare un paio di newsletter da scaricare.Le newsletter selezionate verranno combinate in un pdf e restituite all&#39;utente.




