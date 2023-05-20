---
title: Invio di e-mail all’invio di moduli adattivi
seo-title: Sending Email on Adaptive Form Submission
description: Inviare un messaggio e-mail di conferma per l’invio di moduli adattivi tramite il componente Invia e-mail
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

# Invio di e-mail all’invio di moduli adattivi {#sending-email-on-adaptive-form-submission}

Una delle azioni comuni consiste nell’inviare un messaggio e-mail di conferma al mittente in caso di invio corretto del modulo adattivo. A tal fine, selezioneremo &quot;Invia e-mail&quot; come azione di invio.

Puoi utilizzare il modello di e-mail o semplicemente digitare il corpo dell’e-mail come mostrato in questa schermata.

Osserva la sintassi per inserire i valori dei campi modulo nell’e-mail. Abbiamo anche la possibilità di includere gli allegati del modulo nell’e-mail selezionando la casella di controllo &quot;includi allegati&quot; nelle proprietà di configurazione.

Quando il modulo adattivo viene inviato, il destinatario riceverà un’e-mail.

![InviaE-mail](assets/sendemailaction.gif)

## Configurazioni necessarie {#configurations-needed}

Dovrai configurare il servizio Day CQ Mail. Per configurare questa funzione, fai clic sul browser per [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

La schermata mostra le proprietà di configurazione per il server di posta Adobe.

![mailservice](assets/mailservice.png)

Per provare questa operazione sul server, attenersi alle istruzioni riportate di seguito.

* [Importare le risorse](assets/timeoffrequest.zip) associato a questo articolo in AEM utilizzando il gestore di pacchetti.

* Apri [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Inserisci i dettagli.Assicurati di fornire un indirizzo e-mail valido nel campo e-mail.

* Invia il modulo.
