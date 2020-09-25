---
title: Invio di e-mail per l'invio del modulo adattivo
seo-title: Invio di e-mail per l'invio del modulo adattivo
description: Invio di un messaggio e-mail di conferma per l’invio di un modulo adattivo tramite il componente Invia e-mail
seo-description: Invio di un messaggio e-mail di conferma per l’invio di un modulo adattivo tramite il componente Invia e-mail
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: adaptive-forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
translation-type: tm+mt
source-git-commit: 272e43ee4aeb756a3a1fd0ce55eaca94a9553fa4
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# Invio di e-mail per l&#39;invio del modulo adattivo {#sending-email-on-adaptive-form-submission}

Una delle azioni più comuni consiste nell’inviare un messaggio e-mail di conferma all’autore del modulo adattivo al momento dell’invio corretto. A tal fine, selezioneremo l&#39;azione &quot;Invia e-mail&quot; come azione di invio.

Potete usare il modello e-mail o semplicemente digitare il corpo dell’e-mail come mostrato in questa schermata.

Osservare la sintassi per l&#39;inserimento dei valori dei campi del modulo nel messaggio e-mail.Abbiamo anche la possibilità di includere allegati del modulo nel messaggio e-mail, selezionando la casella di controllo &quot;Includi allegati&quot; nelle proprietà di configurazione.

Quando viene inviato il modulo adattivo, il destinatario riceve un messaggio e-mail.

![SendEmail](assets/sendemailaction.gif)

## Configurazioni necessarie {#configurations-needed}

Sarà necessario configurare il servizio Day CQ Mail. Questa configurazione può essere effettuata puntando il browser a [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

Lo screenshot mostra le proprietà di configurazione per il server di posta Adobe.

![mailservice](assets/mailservice.png)

Per provare a eseguire questa operazione sul server, attenersi alle istruzioni riportate di seguito.

* [Importa le risorse](assets/timeoffrequest.zip) associate a questo articolo in AEM utilizzando il gestore pacchetti.

* Aprire [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Compila i dettagli.Assicurati di fornire un indirizzo e-mail valido nel campo e-mail.

* Inviare il modulo.
