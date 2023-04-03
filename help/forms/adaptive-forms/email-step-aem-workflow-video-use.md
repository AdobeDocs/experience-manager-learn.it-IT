---
title: Utilizzo del passaggio Invia e-mail del Forms Workflow
description: Il passaggio Invia e-mail è stato introdotto in AEM Forms 6.4. Utilizzando questo passaggio è possibile creare processi aziendali o flussi di lavoro che consentono di inviare e-mail con o senza allegati. Il video seguente illustra i passaggi per la configurazione del componente Invia e-mail
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 1%

---

# Utilizzo del passaggio Invia e-mail del Forms Workflow {#using-send-email-step-of-forms-workflow}

Il passaggio Invia e-mail è stato introdotto in AEM Forms 6.4. Utilizzando questo passaggio è possibile creare processi aziendali o flussi di lavoro che consentono di inviare e-mail con o senza allegati. Il video seguente illustra i passaggi necessari per configurare il componente Invia e-mail.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

Come parte di questo articolo, ti guideremo attraverso il seguente caso d’uso:

1. Un utente compila un modulo di richiesta Time Off
1. All’invio del modulo, viene attivato AEM flusso di lavoro
1. Il flusso di lavoro AEM utilizza il componente Invia e-mail per inviare un’e-mail con il DoR come allegato

Prima di utilizzare il passaggio Invia e-mail, assicurati di configurare il servizio Day CQ Mail dal [configMgr](http://localhost:4502/system/console/configMgr). Fornire i valori specifici dell’ambiente

![Configura il servizio di posta CQ Day](assets/mailservice.png)

Come parte delle risorse associate a questo articolo, otterrai quanto segue

1. Modulo adattivo che attiverà il flusso di lavoro all’invio
1. Flusso di lavoro di esempio che invierà un’e-mail con DOR come allegato
1. Bundle OSGi che crea le proprietà dei metadati

Per eseguire il campione sul sistema, procedi come segue:

1. [Distribuzione del bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Scarica e installa il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)Questo bundle contiene il codice per la creazione delle proprietà dei metadati come parte della fase di processo del flusso di lavoro.
1. [Configura il servizio di posta CQ Day](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importa e installa le risorse associate a questo articolo utilizzando il gestore di pacchetti in CRX](assets/emaildoraemformskt.zip)
1. Avvia [modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Compila i campi richiesti e invia.
1. Dovresti ricevere un&#39;e-mail con DocumentOfRecord come allegato

Esplora [modello di flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Guarda il passaggio del processo del flusso di lavoro. Il codice personalizzato associato al passaggio del processo creerà i nomi delle proprietà dei metadati e imposterà i relativi valori dai dati inviati. Questi valori vengono quindi utilizzati dal componente Invia e-mail.

>[!NOTE]
>
>In AEM Forms 6.5 e versioni successive non è necessario questo codice personalizzato per creare le proprietà dei metadati. Utilizza la funzionalità delle variabili in AEM flusso di lavoro

Assicurati che la scheda Allegati del componente Invia e-mail sia configurata in base alla schermata sottostante
![Scheda Invia allegato e-mail](assets/sendemailcomponentconfigure.jpg)Il valore &quot;DOR.pdf&quot; deve corrispondere al valore specificato nel Percorso del documento di record specificato nelle opzioni di invio del modulo adattivo.
