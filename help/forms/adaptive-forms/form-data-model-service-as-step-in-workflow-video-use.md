---
title: Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro
seo-title: Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro
description: A partire da AEM Forms 6.4, ora possiamo utilizzare Form Data Model come parte del flusso di lavoro AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo in Flusso di lavoro AEM.
seo-description: A partire da AEM Forms 6.4, ora possiamo utilizzare Form Data Model come parte del flusso di lavoro AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo in Flusso di lavoro AEM.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: Flusso di lavoro
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: Sviluppo
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 1%

---


# Utilizzo del servizio Form Data Model come passaggio nel flusso di lavoro {#using-form-data-model-service-as-step-in-workflow}

A partire da AEM Forms 6.4, ora possiamo utilizzare Form Data Model come parte del flusso di lavoro AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo in Flusso di lavoro AEM


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Per testare questa funzionalità sul server, segui le istruzioni riportate di seguito
* [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che imposta le proprietà dei metadati.
>!![NOTE]In AEM Forms 6.5 e versioni successive questa funzionalità è disponibile come  [descritto qui](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Imposta tomcat con il file SampleRest.war come descritto [qui](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).Il file war distribuito in Tomcat ha il codice per restituire il punteggio di credito del richiedente. Il punteggio di credito è un numero casuale compreso tra 200 e 800

* [Importa le risorse in AEM utilizzando il gestore di pacchetti](assets/invoke-fdm-as-service-step.zip). Il pacchetto contiene quanto segue:

   * Modello di flusso di lavoro che utilizza il passaggio FDM.
   * Modello dati modulo utilizzato nel passaggio FDM.
   * Modulo adattivo per attivare il flusso di lavoro al momento dell’invio.
* Apri il [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Compila i dettagli e invia. All&#39;invio del modulo viene attivato il [flusso di lavoro dell&#39;applicazione di caricamento](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
Il flusso di lavoro utilizza il componente O diviso per indirizzare l’applicazione all’amministrazione se il punteggio di credito è superiore a 500. Se il punteggio di credito è inferiore a 500, l&#39;applicazione viene indirizzata a cavery
