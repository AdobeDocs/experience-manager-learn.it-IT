---
title: Pagina di ringraziamento invio
seo-title: Submitting To Thank You Page
description: Visualizzare una pagina di ringraziamento all’invio del modulo adattivo
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# Pagina di ringraziamento invio {#submitting-to-thank-you-page}

L’opzione Invia all’endpoint REST trasmette i dati compilati nel modulo a una pagina di conferma configurata come parte della richiesta HTTP GET. Puoi aggiungere il nome dei campi da richiedere. Il formato della richiesta è:

\{fieldName\} = \{parameterName\}. Ad esempio, submitterName è il nome di un campo di un modulo adattivo e submitter è il nome del parametro. Nella pagina di ringraziamento, è possibile accedere al parametro submitter utilizzando request.getParameter(&quot;submitter&quot;) per ottenere il valore del campo del nome del submitter.

`submitterName=submitter`

Nella schermata seguente, stiamo inviando il modulo adattivo per ringraziarti alla pagina che si trova in /content/thankyou. A questa pagina di ringraziamento, stiamo passando 3 attributi di richiesta che contengono i valori dei campi modulo.

![Pagina di ringraziamento](assets/thankyoupage.gif)

Puoi anche inviare all’endpoint esterno tramite POST. A questo scopo, seleziona la casella di controllo &quot;abilita richiesta post&quot; e fornisci l’URL dell’endpoint esterno. Quando si invia il modulo, viene visualizzata una pagina di ringraziamento e l&#39;endpoint POST viene richiamato contemporaneamente.

![Configurazione acquisizione](assets/capture.gif)

Per testare questa funzionalità sul server, attenersi alle istruzioni riportate di seguito:

* Importa [file di risorse associato a questo articolo in AEM utilizzando Gestione pacchetti](assets/submittingtorestendpoint.zip)
* Puntare il browser al [Modulo di richiesta di indisponibilità](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Compila il campo richiesto e invia il modulo
* Dovresti ottenere la pagina di ringraziamento con le informazioni inserite nella pagina
