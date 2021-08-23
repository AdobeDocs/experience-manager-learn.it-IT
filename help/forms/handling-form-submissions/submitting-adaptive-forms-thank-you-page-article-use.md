---
title: Invio della pagina di ringraziamento
seo-title: Invio della pagina di ringraziamento
description: Visualizza una pagina di ringraziamento all’invio del modulo adattivo
seo-description: Visualizza una pagina di ringraziamento all’invio del modulo adattivo
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Moduli adattivi
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---


# Invio della pagina di ringraziamento {#submitting-to-thank-you-page}

L’opzione Invia a endpoint REST passa i dati compilati nel modulo a una pagina di conferma configurata come parte della richiesta HTTP GET. Puoi aggiungere il nome dei campi da richiedere. Il formato della richiesta è:

\{fieldName\} = \{parameterName\}. Ad esempio, submitterName è il nome di un campo modulo adattivo e il nome del parametro è il nome del mittente. Nella pagina di ringraziamento, è possibile accedere al parametro del mittente utilizzando request.getParameter(&quot;submitter&quot;) per ottenere il valore del campo del nome del mittente.

submitterName=submitter

Nella schermata seguente, stiamo inviando il modulo adattivo per ringraziarti la pagina che si trova in /content/grankyou. A questa pagina di ringraziamento, trasmettiamo 3 attributi di richiesta che conterranno i valori del campo modulo.

![grazie](assets/thankyoupage.gif)

È inoltre possibile inviare all’endpoint esterno tramite POST. Per farlo, devi solo selezionare la casella di controllo &quot;abilita richiesta post&quot; e fornire l’URL per l’endpoint esterno. Quando si invia il modulo, si ottiene la pagina di ringraziamento e l’endpoint POST viene richiamato simultaneamente.

![catturare](assets/capture.gif)


Per testare questa funzionalità sul server, segui le istruzioni riportate di seguito:

* Importa in AEM il file [risorse associato a questo articolo utilizzando il gestore di pacchetti](assets/submittingtorestendpoint.zip)
* Posiziona il browser sul [Modulo di richiesta Time Off](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Compila il campo richiesto e invia il modulo
* Dovresti ottenere la pagina di ringraziamento con le tue informazioni popolate sulla pagina

