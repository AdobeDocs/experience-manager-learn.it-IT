---
title: Utilizzo del servizio del modello dati modulo come passaggio nel flusso di lavoro
description: A partire da AEM Forms 6.4, ora è possibile utilizzare il modello dati del modulo come parte del flusso di lavoro AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo nel flusso di lavoro di AEM.
feature: Workflow
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 205
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Utilizzo del servizio del modello dati modulo come passaggio nel flusso di lavoro {#using-form-data-model-service-as-step-in-workflow}

A partire da AEM Forms 6.4, ora è possibile utilizzare il modello dati del modulo come parte del flusso di lavoro AEM. Il video seguente illustra i passaggi necessari per configurare il passaggio Modello dati modulo nel flusso di lavoro di AEM


>[!VIDEO](https://video.tv.adobe.com/v/41741?quality=12&learn=on&captions=ita)

Per testare questa funzionalità sul server, attieniti alle istruzioni seguenti
* [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che imposta le proprietà dei metadati.
>In AEM Forms 6.5 e versioni successive questa funzionalità è disponibile come [descrive qui](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Imposta tomcat con il file SampleRest.war come descritto [qui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=it).Il file .war distribuito in Tomcat ha il codice per restituire il punteggio di credito del candidato. Il credito è un numero casuale compreso tra 200 e 800

* [Importa le risorse in AEM utilizzando Gestione pacchetti](assets/invoke-fdm-as-service-step.zip). Il pacchetto contiene quanto segue:

   * Modello di flusso di lavoro che utilizza il passaggio FDM.
   * Modello dati modulo utilizzato nel passaggio FDM.
   * Modulo adattivo per attivare il flusso di lavoro all’invio.
* Aprire [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Inserisci i dettagli e invia. All&#39;invio del modulo viene attivato il [flusso di lavoro Loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![ flusso di lavoro ](assets/fdm-as-service-step-workflow.PNG).
Il flusso di lavoro utilizza il componente Dividi o per instradare l’applicazione all’amministratore se il punteggio di credito è superiore a 500. Se il punteggio di credito è inferiore a 500, l’applicazione viene instradata a cavery
