---
title: Utilizzo del passaggio Invia e-mail di Forms Workflow
description: Il passaggio Invia e-mail è stato introdotto in AEM Forms 6.4. Utilizzando questo passaggio possiamo creare processi aziendali o flussi di lavoro che ti consentiranno di inviare e-mail con o senza allegati. Il video seguente illustra i passaggi per la configurazione del componente Invia e-mail
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 314
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# Utilizzo del passaggio Invia e-mail di Forms Workflow {#using-send-email-step-of-forms-workflow}

Il passaggio Invia e-mail è stato introdotto in AEM Forms 6.4. Utilizzando questo passaggio possiamo creare processi aziendali o flussi di lavoro che ti consentiranno di inviare e-mail con o senza allegati. Il video seguente illustra i passaggi per la configurazione del componente Invia e-mail.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

Come parte di questo articolo, ti guideremo attraverso il seguente caso d’uso:

1. Un utente compila il modulo di richiesta Time Off (Ferie)
1. All’invio del modulo, viene attivato il flusso di lavoro di AEM
1. Il flusso di lavoro di AEM utilizza il componente Invia e-mail per inviare un’e-mail con il DoR come allegato

Prima di utilizzare il passaggio Invia e-mail, accertati di configurare il servizio di posta Day CQ da [configMgr](http://localhost:4502/system/console/configMgr). Fornisci i valori specifici dell’ambiente

![Configura servizio di posta Day CQ](assets/mailservice.png)

Come parte delle risorse associate a questo articolo, riceverai quanto segue

1. Modulo adattivo che attiverà il flusso di lavoro all’invio
1. Flusso di lavoro di esempio per l&#39;invio di un&#39;e-mail con DOR come allegato
1. Bundle OSGi che crea le proprietà dei metadati

Per eseguire l&#39;esempio sul sistema, eseguire le operazioni seguenti:

1. [Distribuire il bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Scarica e installa il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)Questo bundle contiene il codice per la creazione delle proprietà dei metadati come parte del passaggio del flusso di lavoro.
1. [Configura servizio di posta Day CQ](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importa e installa in CRX le risorse associate a questo articolo utilizzando Gestione pacchetti](assets/emaildoraemformskt.zip)
1. Avvia il [modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Compila i campi obbligatori e invia.
1. Dovresti ricevere un’e-mail con DocumentOfRecord come allegato

Esplora il [modello di flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Dai un’occhiata al passaggio del processo di flusso di lavoro. Il codice personalizzato associato alla fase del processo crea i nomi delle proprietà dei metadati e ne imposta i valori dai dati inviati. Questi valori vengono quindi utilizzati dal componente Invia e-mail.

>[!NOTE]
>
>In AEM Forms 6.5 e versioni successive non è necessario questo codice personalizzato per creare le proprietà dei metadati. Utilizza la funzionalità delle variabili nel flusso di lavoro di AEM

Assicurati che la scheda Allegati del componente Invia e-mail sia configurata come da schermata seguente
![Invia allegato e-mail](assets/sendemailcomponentconfigure.jpg)Il valore &quot;DOR.pdf&quot; deve corrispondere al valore specificato nel percorso del documento record specificato nelle opzioni di invio del modulo adattivo.
