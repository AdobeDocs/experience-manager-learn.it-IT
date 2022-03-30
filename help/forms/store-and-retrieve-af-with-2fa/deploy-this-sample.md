---
title: Distribuire il campione
description: Scarica il caso d’uso in esecuzione sull’istanza AEM Forms locale
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---

# Distribuire il campione

Per far funzionare questo caso d&#39;uso sul tuo sistema, segui le seguenti istruzioni:

>[!NOTE]
>Si presume che AEM Forms sia in esecuzione sulla porta 4502.


## Crea database

Questo esempio utilizza il database MySQL per memorizzare i dati del modulo adattivo. Sarà necessario creare il [schema di database importando il file di schema](assets/data-base-schema.sql) in Workbench di MySQL.

## Crea origine dati

È necessario creare un’origine dati denominata **StoreAndRetrieveAfData**. Il codice nel bundle OSGi utilizza questo nome dell&#39;origine dati

## Crea modello dati modulo

È necessario creare il modello dati modulo in base a questa origine dati denominata **StoreAndRetrieveAfData**. Questo modello di dati del modulo viene utilizzato per recuperare il numero di telefono cellulare associato all’ID dell’applicazione. Il modello dati del modulo può essere [scaricato da qui.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Creare un account sviluppatore con nexmo

Crea un account sviluppatore con [Nexmo](https://dashboard.nexmo.com/) per l’invio e la verifica dei codici OTP. Prendi nota della chiave API e della chiave segreto API. Il modello per l’origine dati e i dati del modulo è già stato creato per l’utente a fronte di questo servizio e viene incluso con le risorse menzionate nel passaggio precedente.

## Distribuire i seguenti bundle OSGi

Distribuisci il bundle che ha il [codice per memorizzare e recuperare dati dal database](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
Scarica e decomprimi il file [sviluppo con serviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Distribuisci il file DevelopingWithServiceUser.jar utilizzando la console web Felix.

## Distribuire la libreria client

L’esempio utilizza 2 librerie client. Importa questi [librerie client](assets/client-libraries.zip) in AEM.

## Importare il modello di modulo adattivo personalizzato

I moduli di esempio utilizzati in questa demo si basano su un modello personalizzato. Importa [modello personalizzato in AEM](assets/custom-template-with-page-component.zip)

## Importare i moduli adattivi di esempio

È necessario importare in AEM i 2 moduli che costituiscono questo esempio. I moduli di esempio possono essere [scaricato da qui](assets/sample-forms.zip)

Apri [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in modalità di modifica. Specifica i valori Chiave API e Segreto API nei campi appropriati nel modulo adattivo.

## Verificare la soluzione

Visualizzare in anteprima l&#39;anteprima del [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Inserisci il tuo numero di cellulare, compreso il codice del paese , compila i tuoi dettagli utente e aggiungi alcuni allegati. Fai clic sul pulsante &quot;Salva ed esci&quot; per salvare il modulo adattivo e i relativi allegati


## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
