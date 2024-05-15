---
title: Utilizzo del Servizio modello dati modulo come passaggio nel flusso di lavoro AEM 6.5
description: AEM Forms 6.5 ha introdotto la possibilità di creare variabili nel flusso di lavoro AEM. Con questa nuova funzionalità, usare "Invoke Form Data Model Service" nel flusso di lavoro AEM è diventato molto semplice. Il video seguente illustra i passaggi necessari per utilizzare il servizio Richiama modello dati modulo nel flusso di lavoro AEM.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 239
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---

# Utilizzo del Servizio modello dati modulo come passaggio nel flusso di lavoro AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partire da AEM Forms 6.4, ora è possibile utilizzare il Servizio modello dati modulo come parte del flusso di lavoro AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo nel flusso di lavoro AEM

>La funzionalità illustrata in questo video richiede AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

Per testare questa funzionalità sul server, attieniti alle istruzioni seguenti

* Configurare tomcat con il file SampleRest.war come descritto [qui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Il file .war distribuito in Tomcat ha il codice per restituire il punteggio di credito del richiedente.Il punteggio di credito è un numero casuale compreso tra 200 e 800

* [Importare le risorse in AEM utilizzando Gestione pacchetti](assets/aem65-loanapplication.zip)
* La confezione contiene quanto segue:

   * Modello di flusso di lavoro che utilizza il passaggio FDM.
   * Modello dati modulo utilizzato nel passaggio FDM.
   * Modulo adattivo per attivare il flusso di lavoro all’invio.
* Apri [ModuloApplicazioneIpoteca](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Inserisci i dettagli e invia. All’invio del modulo [flusso di lavoro per richiesta di prestito](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) viene attivato.

![ workflow ](assets/invokefdm651.PNG).
Il flusso di lavoro utilizza il componente Dividi o per instradare l’applicazione all’amministratore se il punteggio di credito è superiore a 500. Se il punteggio di credito è inferiore a 500, l’applicazione viene instradata a cavery.
