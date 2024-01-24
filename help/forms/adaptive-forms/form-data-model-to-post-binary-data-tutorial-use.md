---
title: Utilizzo Del Modello Dati Modulo Per Pubblicare Dati Binari
description: Pubblicazione di dati binari in AEM DAM tramite il modello dati modulo
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 106
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Utilizzo Del Modello Dati Modulo Per Pubblicare Dati Binari{#using-form-data-model-to-post-binary-data}

A partire da AEM Forms 6.4, ora è possibile richiamare il Servizio modello dati modulo come passaggio nel flusso di lavoro AEM. Questo articolo illustra un caso d’uso esemplificativo per la pubblicazione di documenti di record tramite il servizio modello dati modulo.

Il caso d’uso è il seguente:

1. Un utente compila e invia un modulo adattivo.
1. Il modulo adattivo è configurato per generare un documento di record.
1. All’invio di questi moduli adattivi, viene attivato il flusso di lavoro AEM che utilizzerà il servizio Richiama modello dati modulo per eseguire il POST del documento record in AEM DAM.

![posttodam](assets/posttodamshot1.png)

Scheda Modello dati modulo - Proprietà

Nella scheda Input servizio vengono mappati i seguenti elementi

* file (l’oggetto binario che deve essere memorizzato) con la proprietà DOR.pdf relativa al payload. Ciò significa che quando il modulo adattivo viene inviato, il documento di record generato viene memorizzato in un file denominato DOR.pdf relativo al payload del flusso di lavoro.**Assicurati che il file DOR.pdf sia lo stesso fornito durante la configurazione della proprietà di invio del modulo adattivo.**

* fileName: nome con cui l’oggetto binario viene memorizzato in DAM. Pertanto, desideri che questa proprietà venga generata in modo dinamico, in modo che ogni fileName sia univoco per ogni invio. A questo scopo abbiamo utilizzato il passaggio del processo nel flusso di lavoro per creare una proprietà di metadati denominata nomefile e impostarne il valore sulla combinazione di Nome membro e Numero account della persona che invia il modulo. Ad esempio, se il nome del membro della persona è John Jacobs e il suo numero di account è 9846, il nome file sarà John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Input servizio

>[!NOTE]
>
>Suggerimenti per la risoluzione dei problemi: se per qualche motivo il file DOR.pdf non viene creato in DAM, reimposta le impostazioni di autenticazione dell’origine dati facendo clic su [qui](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Si tratta delle impostazioni di autenticazione dell’AEM, che per impostazione predefinita è amministratore/amministratore.

Per testare questa funzionalità sul server, segui i passaggi indicati di seguito:

1.[Distribuire il bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)Questo bundle OSGI personalizzato viene utilizzato per creare la proprietà dei metadati e impostarne il valore dai dati del modulo inviati.

1. [Importare le risorse](assets/postdortodam.zip) associato a questo articolo nell’AEM utilizzando Gestione pacchetti.Si otterrà quanto segue

   1. Modello flusso di lavoro
   1. Modulo adattivo configurato per l’invio al flusso di lavoro AEM
   1. Origine dati configurata per utilizzare il file PostToDam.JSON
   1. Modello dati modulo che utilizza l’origine dati

1. Punta il tuo [per aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Compila il modulo e invia.
1. Se il documento di record viene creato e memorizzato, controlla l’applicazione Assets.


[File Swagger](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) utilizzato nella creazione dell&#39;origine dati è disponibile come riferimento
