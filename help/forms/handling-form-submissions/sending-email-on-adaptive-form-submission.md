---
title: Invio di e-mail con invio di moduli adattivi
seo-title: Sending Email on Adaptive Form Submission
description: Invia e-mail di conferma per l’invio di moduli adattivi utilizzando il componente Invia e-mail
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 3%

---

# Invio di e-mail con invio di moduli adattivi {#sending-email-on-adaptive-form-submission}

Una delle azioni comuni consiste nell’inviare un messaggio e-mail di conferma al mittente in seguito all’invio del modulo adattivo. A tal fine, selezioneremo l’azione &quot;Invia e-mail&quot; come azione di invio.

Puoi utilizzare il modello e-mail o semplicemente digitare il corpo dell’e-mail come mostrato nella schermata seguente.

Osserva la sintassi per inserire i valori del campo modulo nell&#39;e-mail. Abbiamo anche la possibilità di includere gli allegati del modulo nell&#39;e-mail, selezionando la casella di controllo &quot;include allegati&quot; nelle proprietà di configurazione.

All’invio del modulo adattivo, il destinatario riceverà un’e-mail.

![InviaE-mail](assets/sendemailaction.gif)

## Configurazioni necessarie {#configurations-needed}

Sarà necessario configurare il servizio Day CQ Mail. Questa configurazione può essere effettuata puntando il browser verso [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

La schermata mostra le proprietà di configurazione per adobe mail server.

![mailservice](assets/mailservice.png)

Per provare questo sul tuo server segui queste istruzioni:

* [Importare le risorse](assets/timeoffrequest.zip) associato a questo articolo in AEM utilizzando il gestore dei pacchetti.

* Apri [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Compila i dettagli.Assicurati di fornire un indirizzo e-mail valido nel campo e-mail.

* Invia il modulo.
