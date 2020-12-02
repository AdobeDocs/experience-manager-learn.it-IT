---
title: Utilizzo di Invia passaggio e-mail del Forms Workflow
seo-title: Utilizzo di Invia passaggio e-mail del Forms Workflow
description: Il passaggio Invia e-mail è stato introdotto in  AEM Forms 6.4. Questo passaggio consente di creare processi aziendali o flussi di lavoro che consentono di inviare e-mail con o senza allegati. Il seguente video illustra i passaggi necessari per configurare il componente Invia e-mail
seo-description: Il passaggio Invia e-mail è stato introdotto in  AEM Forms 6.4. Questo passaggio consente di creare processi aziendali o flussi di lavoro che consentono di inviare e-mail con o senza allegati. Il seguente video illustra i passaggi necessari per configurare il componente Invia e-mail
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---


# Utilizzo dell&#39;opzione Invia e-mail passaggio del Forms Workflow {#using-send-email-step-of-forms-workflow}

Il passaggio Invia e-mail è stato introdotto in  AEM Forms 6.4. Questo passaggio consente di creare processi aziendali o flussi di lavoro che consentono di inviare e-mail con o senza allegati. Il seguente video illustra i passaggi necessari per configurare il componente Invia e-mail.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

Come parte di questo articolo, vi illustreremo i seguenti casi di utilizzo:

1. Un utente compila il modulo di richiesta del tempo di disattivazione
1. All&#39;invio del modulo, viene attivato AEM flusso di lavoro
1. Il flusso di lavoro AEM utilizza il componente Invia e-mail per inviare un messaggio e-mail con il DoR come allegato

Prima di utilizzare il passaggio Invia e-mail, assicurarsi di configurare Day CQ Mail Service da [configMgr](http://localhost:4502/system/console/configMgr). Fornire i valori specifici dell&#39;ambiente

![Configura servizio di posta CQ Day](assets/mailservice.png)

Come parte delle risorse associate a questo articolo, vengono fornite le seguenti informazioni

1. Modulo adattivo che attiverà il flusso di lavoro all’invio
1. Esempio di flusso di lavoro che invia un messaggio e-mail con DOR come allegato
1. Pacchetto OSGi che crea le proprietà dei metadati

Per eseguire l&#39;esempio sul sistema, effettuare le seguenti operazioni:

1. [Distribuzione del bundle Developingwithservice](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Download e installazione del ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)bundle setvalueQuesto bundle contiene il codice per la creazione delle proprietà dei metadati come parte del processo del flusso di lavoro.
1. [Configura servizio di posta CQ Day](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importare e installare le risorse associate a questo articolo utilizzando il gestore pacchetti in CRX](assets/emaildoraemformskt.zip)
1. Avviare il [modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Compila i campi richiesti e invia.
1. Dovreste ricevere un&#39;e-mail con DocumentOfRecord come allegato

Esplora il modello di flusso di lavoro [](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Guardate il passaggio del flusso di lavoro. Il codice personalizzato associato al passaggio del processo creerà i nomi delle proprietà dei metadati e ne imposta i valori dai dati inviati. Questi valori vengono quindi utilizzati dal componente Invia e-mail.

>[!NOTE]
>
>In  AEM Forms 6.5 e versioni successive non è necessario questo codice personalizzato per creare le proprietà dei metadati. Utilizzate la funzionalità delle variabili in AEM Workflow

Accertatevi che la scheda Allegati del componente Invia e-mail sia configurata in base alla schermata seguente
![Invia scheda allegati e-mail](assets/sendemailcomponentconfigure.jpg)Il valore &quot;DOR.pdf&quot; deve corrispondere al valore specificato nel Percorso del documento di registrazione specificato nelle opzioni di invio del modulo adattivo.

