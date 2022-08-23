---
title: Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro AEM 6.5
description: AEM Forms 6.5 ha introdotto la possibilità di creare variabili nel flusso di lavoro AEM. Con questa nuova funzionalità che utilizza il "Invoke Form Data Model Service" in AEM Workflow è diventato molto semplice. Il video seguente illustra i passaggi necessari per l’utilizzo di Invoke Form Data Model Service in AEM flusso di lavoro.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partire da AEM Forms 6.4, ora è possibile utilizzare il servizio Form Data Model come parte del flusso di lavoro di AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo in AEM Flusso di lavoro

>!![NOTE]La funzionalità dimostrata in questo video richiede AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Per testare questa funzionalità sul server, segui le istruzioni riportate di seguito

* Imposta tomcat con il file SampleRest.war come descritto [qui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Il file di guerra distribuito in Tomcat ha il codice per restituire il punteggio di credito del richiedente.Il punteggio di credito è un numero casuale compreso tra 200 e 800

* [ Importare le risorse in AEM utilizzando Gestione pacchetti](assets/aem65-loanapplication.zip)
* Il pacchetto contiene quanto segue:

   * Modello di flusso di lavoro che utilizza il passaggio FDM.
   * Modello dati modulo utilizzato nel passaggio FDM.
   * Modulo adattivo per attivare il flusso di lavoro al momento dell’invio.
* Apri [ModuloApplicazioneMutui](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Compila i dettagli e invia. Sul modulo di invio del [flusso di lavoro dell’applicazione del prestito](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) viene attivato.

![ workflow ](assets/invokefdm651.PNG).
Il flusso di lavoro utilizza il componente O diviso per indirizzare l’applicazione all’amministrazione se il punteggio di credito è superiore a 500. Se il punteggio di credito è inferiore a 500, l&#39;applicazione viene indirizzata a cavery.
