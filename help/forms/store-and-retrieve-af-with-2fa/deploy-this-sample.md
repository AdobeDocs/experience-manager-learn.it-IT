---
title: Distribuzione dell’esempio
description: Richiedi l'esecuzione di un caso d'uso nell'istanza AEM Forms  locale
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---



# Distribuzione dell’esempio

Per fare in modo che questo caso d&#39;uso funzioni sul vostro sistema, seguite le seguenti istruzioni:

>[!NOTE]
>Si presume che sia in esecuzione  AEM Forms sulla porta 4502.


## Crea database

Questo esempio utilizza il database MySQL per memorizzare i dati del modulo adattivo. Sarà necessario creare lo schema di database [importando il file di schema](assets/data-base-schema.sql) in Workbench MySQL.

## Crea origine dati

È necessario creare un&#39;origine dati denominata **StoreAndRetrieveAfData**. Il codice nel bundle OSGi utilizza questo nome origine dati

## Crea modello dati modulo

È necessario creare il modello dati del modulo in base a questa origine dati denominata **StoreAndRetrieveAfData**. Questo modello di dati del modulo viene utilizzato per recuperare il numero di telefono cellulare associato all&#39;ID applicazione. Il modello dati modulo può essere [scaricato da qui.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Creazione di un account sviluppatore con

Create un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/) per l&#39;invio e la verifica dei codici OTP. Prendete nota della chiave API e della chiave segreta API. Il modello di origine dati e i dati del modulo sono già stati creati per l&#39;utente a fronte di questo servizio e sono inclusi con le risorse menzionate nel passaggio precedente.

## Distribuzione dei seguenti bundle OSGi

Distribuire il bundle con il codice [per memorizzare e recuperare i dati dal database](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
Distribuire il pacchetto [DevelopingWithServiceUser](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).

## Distribuzione della libreria client

Nell&#39;esempio vengono utilizzate 2 librerie client. Importa queste [librerie client](assets/client-libraries.zip) in AEM.

## Importare il modello di modulo adattivo personalizzato

I moduli di esempio utilizzati in questa demo si basano su un modello personalizzato. Importa il modello personalizzato [in AEM](assets/custom-template-with-page-component.zip)

## Importare i moduli adattivi di esempio

È necessario importare in AEM i due moduli che compongono questo esempio. I moduli di esempio possono essere [scaricati da qui](assets/sample-forms.zip)

Aprire il modulo [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in modalità di modifica. Specificate i valori Chiave API e Segreto API nei campi appropriati del modulo adattivo.

## Verificare la soluzione

Visualizzare l&#39;anteprima di [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Inserisci il tuo numero di cellulare incluso il codice del paese, compila i tuoi dettagli utente e aggiungi alcuni allegati. Fare clic sul pulsante &quot;Salva ed esci&quot; per salvare il modulo adattivo e i relativi allegati


## Dimostrazione del caso di utilizzo

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
