---
title: Utilizzo Del Modello Dati Modulo Per Pubblicare Dati Binari
seo-title: Utilizzo Del Modello Dati Modulo Per Pubblicare Dati Binari
description: Inserimento di dati binari in AEM DAM tramite il modello dati modulo
seo-description: Inserimento di dati binari in AEM DAM tramite il modello dati modulo
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---


# Utilizzo del modello dati modulo per inviare dati binari{#using-form-data-model-to-post-binary-data}

A partire da  AEM Forms 6.4, ora è possibile richiamare il servizio Form Data Model Service come passaggio AEM Workflow. Questo articolo illustra un esempio di utilizzo per la pubblicazione del documento di registrazione tramite il servizio Form Data Model Service.

Il caso di utilizzo è il seguente:

1. Un utente compila e invia il modulo adattivo.
1. Il modulo adattivo è configurato per generare il documento di registrazione.
1. Quando si inviano questi moduli adattivi, viene attivato AEM Flusso di lavoro, che utilizzerà il servizio Richiama Form Data Model per POST del documento di record a AEM DAM.

![posttodam](assets/posttodamshot1.png)

Scheda Modello dati modulo - Proprietà

Nella scheda Input servizio viene mappato quanto segue

* file(L&#39;oggetto binario che deve essere memorizzato) con la proprietà DOR.pdf relativa al payload. Ciò significa che, quando viene inviato il modulo adattivo, il documento di registrazione generato verrà memorizzato in un file denominato DOR.pdf relativo al payload del flusso di lavoro.**Assicurarsi che il file DOR.pdf sia lo stesso fornito durante la configurazione della proprietà di invio del modulo adattivo.**

* fileName - Nome utilizzato per memorizzare l&#39;oggetto binario in DAM. Pertanto, si desidera generare questa proprietà in modo dinamico, in modo che ogni fileName sia univoco per invio. A questo scopo, nel flusso di lavoro è stata utilizzata la procedura per creare la proprietà metadati denominata nomefile e impostarne il valore sulla combinazione di Nome membro e Numero account della persona che invia il modulo. Ad esempio, se il nome membro della persona è John Jacobs e il suo numero di conto è 9846, il nome del file sarà John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Input servizio

>[!NOTE]
>
>Suggerimenti per la ripresa di problemi - Se per qualche motivo il file DOR.pdf non viene creato in DAM, reimpostare le impostazioni di autenticazione dell&#39;origine dati facendo clic su [qui](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Queste sono le impostazioni di autenticazione AEM, che per impostazione predefinita sono admin/admin.

Per testare questa funzionalità sul server, attenetevi alla procedura indicata di seguito:

1.[Implementare il bundle Developingwithservice_user](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Scaricate e distribuite il bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue.Questo bundle OSGI personalizzato viene utilizzato per creare la proprietà dei metadati e impostarne il valore dai dati del modulo inviati.

1. [Importa le ](assets/postdortodam.zip) risorse associate a questo articolo in AEM utilizzando il gestore pacchetti. Verranno fornite le informazioni seguenti

   1. Modello flusso di lavoro
   1. Modulo adattivo configurato per l’invio al flusso di lavoro AEM
   1. Origine dati configurata per l&#39;utilizzo del file PostToDam.JSON
   1. Modello dati modulo che utilizza l&#39;origine dati

1. Posizionare il [browser per aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Compila il modulo e invia.
1. Controlla l’applicazione Risorse se il documento di registrazione viene creato e memorizzato.


[Il ](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) file Swagger utilizzato per creare l&#39;origine dati è disponibile per il riferimento
