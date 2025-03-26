---
title: Distribuire l’esempio
description: Ottieni un caso d’uso in esecuzione sull’istanza AEM Forms locale
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# Distribuire l’esempio

Per fare in modo che questo caso d’uso funzioni sul tuo sistema, segui le seguenti istruzioni:

>[!NOTE]
>Si presume che AEM Forms sia in esecuzione sulla porta 4502.


## Crea database

In questo esempio si utilizza il database MySQL per memorizzare i dati del modulo adattivo. Sarà necessario creare lo schema di database [ importando il file di schema](assets/data-base-schema.sql) in MySQL Workbench.

## Crea origine dati

È necessario creare un&#39;origine dati in pool di connessione Apache Sling denominata **StoreAndRetrieveAfData** che punta allo schema di database creato nel passaggio precedente. Il codice nel bundle OSGi utilizza questo nome di origine dati.

## Crea modello dati modulo

È necessario creare il modello dati modulo in base a questa origine dati denominata **StoreAndRetrieveAfData**. Questo modello di dati modulo viene utilizzato per recuperare il numero di telefono cellulare associato all’ID applicazione. Il modello dati del modulo può essere [scaricato da qui.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Creare un account sviluppatore con nexmo

Crea un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/) per inviare e verificare i codici OTP. Prendi nota della chiave API e della chiave segreta API. L’origine dati e il modello dati del modulo sono già stati creati per te in base a questo servizio e sono inclusi nelle risorse menzionate nel passaggio precedente.

## Distribuisci i seguenti bundle OSGi

Distribuisci il bundle con [codice per archiviare e recuperare dati dal database](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
Scarica e decomprimi [develingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Distribuisci il file DevelopingWithServiceUser.jar utilizzando la console web Felix.

## Distribuire la libreria client

Nell&#39;esempio vengono utilizzate 2 librerie client. Importa queste [librerie client](assets/store-af-with-attachments-client-lib.zip) in AEM.

## Importare il modello di modulo adattivo personalizzato

I moduli di esempio utilizzati in questa demo sono basati su un modello personalizzato. Importa il [modello personalizzato in AEM](assets/custom-template-with-page-component.zip)

## Importare i moduli adattivi di esempio

I 2 moduli che compongono questo esempio devono essere importati in AEM. I moduli di esempio possono essere [scaricati da qui](assets/sample-forms.zip)

Apri [ModuloAccount](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in modalità di modifica. Specifica i valori Chiave API vonage e Segreto API nei campi appropriati del modulo adattivo.

## Testare la soluzione

Visualizza l&#39;anteprima di [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Inserisci il numero di cellulare, compreso il prefisso internazionale, inserisci i dettagli utente e aggiungi alcuni allegati. Fai clic sul pulsante &quot;Salva ed esci&quot; per salvare il modulo adattivo e i relativi allegati


## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
