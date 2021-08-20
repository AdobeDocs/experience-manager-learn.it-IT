---
title: Utilizzo Del Modello Dati Modulo Per Pubblicare Dati Binari
description: Inserimento di dati binari AEM DAM utilizzando il modello dati modulo
feature: Flusso di lavoro
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 1%

---


# Utilizzo Del Modello Dati Modulo Per Pubblicare Dati Binari{#using-form-data-model-to-post-binary-data}

A partire da AEM Forms 6.4, ora possiamo invocare il servizio Form Data Model come passaggio in AEM flusso di lavoro. Questo articolo illustra un esempio di caso d’uso per la pubblicazione di un documento di record tramite il servizio Form Data Model.

Il caso d’uso è il seguente:

1. Un utente compila e invia il modulo adattivo.
1. Il modulo adattivo è configurato per generare il documento di record.
1. All’invio di questo modulo adattivo, viene attivato AEM flusso di lavoro che utilizzerà il servizio Richiama Form Data Model per POST del documento di record a AEM DAM.

![posttodam](assets/posttodamshot1.png)

Scheda Modello dati modulo - Proprietà

Nella scheda Input servizio viene eseguito il mapping dei seguenti elementi

* file(L&#39;oggetto binario che deve essere memorizzato) con la proprietà DOR.pdf relativa al payload. Ciò significa che, quando il Modulo adattivo viene inviato, il Documento di record generato verrà memorizzato in un file denominato DOR.pdf relativo al payload del flusso di lavoro.**Assicurati che DOR.pdf sia lo stesso fornito durante la configurazione della proprietà di invio del modulo adattivo.**

* fileName - Questo è il nome con cui l&#39;oggetto binario verrà memorizzato in DAM. Pertanto, desideri generare questa proprietà in modo dinamico, in modo che ogni fileName sia univoco per ogni invio. A questo scopo abbiamo utilizzato la fase di processo nel flusso di lavoro per creare la proprietà dei metadati denominata nomefile e impostarne il valore sulla combinazione di Nome membro e Numero account della persona che invia il modulo. Ad esempio, se il nome del membro della persona è John Jacobs e il suo numero di conto è 9846, il nome del file sarà John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Input servizio

>[!NOTE]
>
>Suggerimenti per la risoluzione dei problemi - Se per qualche motivo il file DOR.pdf non viene creato in DAM, reimposta le impostazioni di autenticazione dell’origine dati facendo clic su [qui](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Queste sono le impostazioni di autenticazione AEM, che per impostazione predefinita è admin/admin.

Per testare questa funzionalità sul server, segui i passaggi indicati di seguito:

1.[Distribuisci il bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo bundle OSGI personalizzato viene utilizzato per creare la proprietà dei metadati e imposta il suo valore dai dati del modulo inviati.

1. [Importa le ](assets/postdortodam.zip) risorse associate a questo articolo in AEM utilizzando il gestore dei pacchetti. Ottieni quanto segue

   1. Modello flusso di lavoro
   1. Modulo adattivo configurato per l’invio al flusso di lavoro AEM
   1. Origine dati configurata per l’utilizzo del file PostToDam.JSON
   1. Modello dati modulo che utilizza l’origine dati

1. Puntare il browser [per aprire Modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Compila il modulo e invia.
1. Controlla l&#39;applicazione Assets se il Documento di record viene creato e memorizzato.


[Il file Swagger ](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) utilizzato per la creazione dell&#39;origine dati è disponibile per il riferimento
