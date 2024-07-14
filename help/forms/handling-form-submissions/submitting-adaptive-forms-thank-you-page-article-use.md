---
title: Pagina di ringraziamento invio
description: Visualizzare una pagina di ringraziamento all’invio del modulo adattivo
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 51
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# Pagina di ringraziamento invio {#submitting-to-thank-you-page}

L’opzione Invia all’endpoint REST trasmette i dati compilati nel modulo a una pagina di conferma configurata come parte della richiesta HTTP GET. Puoi aggiungere il nome dei campi da richiedere. Il formato della richiesta è:

\{fieldName\} = \{parameterName\}. Ad esempio, submitterName è il nome di un campo di un modulo adattivo e submitter è il nome del parametro. Nella pagina di ringraziamento, è possibile accedere al parametro submitter utilizzando request.getParameter(&quot;submitter&quot;) per ottenere il valore del campo del nome del submitter.

`submitterName=submitter`

Nella schermata seguente, stiamo inviando il modulo adattivo per ringraziarti alla pagina che si trova in /content/thankyou. A questa pagina di ringraziamento, stiamo passando 3 attributi di richiesta che contengono i valori dei campi modulo.

![Pagina di ringraziamento](assets/thankyoupage.gif)

Puoi anche inviare all’endpoint esterno tramite POST. A questo scopo, seleziona la casella di controllo &quot;abilita richiesta post&quot; e fornisci l’URL dell’endpoint esterno. Quando si invia il modulo, viene visualizzata una pagina di ringraziamento e l&#39;endpoint POST viene richiamato contemporaneamente.

![Acquisisci configurazione](assets/capture.gif)

Per testare questa funzionalità sul server, attenersi alle istruzioni riportate di seguito:

* Importa il file [assets associato a questo articolo in AEM utilizzando Gestione pacchetti](assets/submittingtorestendpoint.zip)
* Puntare il browser al [modulo di richiesta di indisponibilità](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Compila il campo richiesto e invia il modulo
* Dovresti ottenere la pagina di ringraziamento con le informazioni inserite nella pagina
