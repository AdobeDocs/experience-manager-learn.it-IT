---
title: Invio di e-mail con invio di moduli adattivi
seo-title: Invio di e-mail con invio di moduli adattivi
description: Invia e-mail di conferma per l’invio di moduli adattivi utilizzando il componente Invia e-mail
seo-description: Invia e-mail di conferma per l’invio di moduli adattivi utilizzando il componente Invia e-mail
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Moduli adattivi
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 1%

---


# Invio di e-mail con invio di moduli adattivi {#sending-email-on-adaptive-form-submission}

Una delle azioni comuni consiste nell’inviare un messaggio e-mail di conferma al mittente in seguito all’invio del modulo adattivo. A tal fine, selezioneremo l’azione &quot;Invia e-mail&quot; come azione di invio.

Puoi utilizzare il modello e-mail o semplicemente digitare il corpo dell’e-mail come mostrato nella schermata seguente.

Osserva la sintassi per inserire i valori del campo modulo nell&#39;e-mail. Abbiamo anche la possibilità di includere gli allegati del modulo nell&#39;e-mail, selezionando la casella di controllo &quot;include allegati&quot; nelle proprietà di configurazione.

All’invio del modulo adattivo, il destinatario riceverà un’e-mail.

![InviaE-mail](assets/sendemailaction.gif)

## Configurazioni necessarie {#configurations-needed}

Sarà necessario configurare il servizio Day CQ Mail. Questo può essere configurato puntando il browser su [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

La schermata mostra le proprietà di configurazione per adobe mail server.

![mailservice](assets/mailservice.png)

Per provare questo sul tuo server segui queste istruzioni:

* [Importa le ](assets/timeoffrequest.zip) risorse associate a questo articolo in AEM utilizzando il gestore dei pacchetti.

* Apri [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Compila i dettagli.Assicurati di fornire un indirizzo e-mail valido nel campo e-mail.

* Invia il modulo.
