---
title: Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro AEM 6.5
seo-title: Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro AEM 6.5
description: AEM Forms 6.5 ha introdotto la possibilità di creare variabili nel flusso di lavoro AEM. Con questa nuova funzionalità che utilizza il "Invoke Form Data Model Service" in AEM Workflow è diventato molto semplice. Il video seguente illustra i passaggi necessari per utilizzare il servizio Invoke Form Data Model in AEM Workflow.
seo-description: AEM Forms 6.5 ha introdotto la possibilità di creare variabili nel flusso di lavoro AEM. Con questa nuova funzionalità che utilizza il "Invoke Form Data Model Service" in AEM Workflow è diventato molto semplice. Il video seguente illustra i passaggi necessari per utilizzare il servizio Invoke Form Data Model in AEM Workflow.
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---


# Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partire da AEM Forms 6.4, ora possiamo utilizzare il servizio Modello dati modulo come parte del flusso di lavoro AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo in Flusso di lavoro AEM

>!![NOTE]La funzionalità mostrata in questo video richiede AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Per testare questa funzionalità sul server, segui le istruzioni riportate di seguito

* Imposta tomcat con il file SampleRest.war come descritto [qui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Il file war distribuito in Tomcat ha il codice per restituire il punteggio di credito del richiedente.Il punteggio di credito è un numero casuale tra 200 e 800

* [ Importare le risorse in AEM utilizzando il gestore dei pacchetti](assets/aem65-loanapplication.zip)
* Il pacchetto contiene quanto segue:

   * Modello di flusso di lavoro che utilizza il passaggio FDM.
   * Modello dati modulo utilizzato nel passaggio FDM.
   * Modulo adattivo per attivare il flusso di lavoro al momento dell’invio.
* Apri il [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Compila i dettagli e invia. All&#39;invio del modulo viene attivato il [flusso di lavoro dell&#39;applicazione di caricamento](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![ workflow ](assets/invokefdm651.PNG).
Il flusso di lavoro utilizza il componente O diviso per indirizzare l’applicazione all’amministrazione se il punteggio di credito è superiore a 500. Se il punteggio di credito è inferiore a 500, l&#39;applicazione viene indirizzata a cavery.
