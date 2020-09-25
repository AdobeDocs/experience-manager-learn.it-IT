---
title: Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro
seo-title: Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro
description: A partire da  AEM Forms 6.4, ora è possibile utilizzare il modello dati modulo come parte AEM flusso di lavoro. Il seguente video illustra i passaggi necessari per configurare il passaggio Modello dati modulo in AEM Flusso di lavoro.
seo-description: A partire da  AEM Forms 6.4, ora è possibile utilizzare il modello dati modulo come parte AEM flusso di lavoro. Il seguente video illustra i passaggi necessari per configurare il passaggio Modello dati modulo in AEM Flusso di lavoro.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro {#using-form-data-model-service-as-step-in-workflow}

A partire da  AEM Forms 6.4, ora è possibile utilizzare il modello dati modulo come parte AEM flusso di lavoro. Il seguente video illustra i passaggi necessari per configurare il passaggio Modello dati modulo in AEM Flusso di lavoro


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Per testare questa funzionalità sul server, segui le istruzioni riportate di seguito
* [Scaricate e distribuite il bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)setvalue. Si tratta del pacchetto OSGI personalizzato che imposta le proprietà dei metadati.
>!![NOTE]In  AEM Forms 6.5 e versioni successive, questa funzionalità è disponibile come [descritto di seguito](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Setup tomcat con il file SampleRest.war come descritto [qui](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).Il file di guerra distribuito in Tomcat ha il codice per restituire il punteggio di credito del richiedente. Il punteggio di credito è un numero casuale compreso tra 200 e 800

* [Importa le risorse in AEM utilizzando il gestore](assets/invoke-fdm-as-service-step.zip)pacchetti. Il pacchetto contiene quanto segue:

   * Modello di flusso di lavoro che utilizza il passaggio FDM.
   * Modello dati modulo utilizzato nel passaggio FDM.
   * Modulo adattivo per attivare il flusso di lavoro all’invio.
* Aprire [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Compila i dettagli e invia. All&#39;invio del modulo viene attivato il flusso di lavoro [dell&#39;applicazione di](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) prestito.

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
Il flusso di lavoro utilizza il componente O Dividi per indirizzare l’applicazione all’amministratore se il punteggio di credito è superiore a 500. Se il punteggio di credito è inferiore a 500, l&#39;applicazione viene indirizzata alla cavery
