---
title: Impostazione del valore dell’elemento dati Json nel flusso di lavoro di AEM Forms
description: Poiché un modulo adattivo viene instradato a utenti diversi in AEM Workflow, è necessario nascondere o disabilitare alcuni campi o pannelli in base alla persona che rivede il modulo. Per soddisfare questi casi d’uso, in genere si imposta il valore di un campo nascosto. In base al valore di questo campo nascosto, le regole business possono essere create per nascondere/disabilitare i pannelli o i campi appropriati.
feature: Adaptive Forms
version: Experience Manager 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 126
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---

# Impostazione del valore dell’elemento dati JSON nel flusso di lavoro AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Poiché un modulo adattivo viene instradato a utenti diversi in AEM Workflow, è necessario nascondere o disabilitare alcuni campi o pannelli in base alla persona che rivede il modulo. Per soddisfare questi casi d’uso, in genere si imposta il valore di un campo nascosto. In base al valore di questo campo nascosto, le regole business possono essere create per nascondere/disabilitare i pannelli o i campi appropriati.

![Impostazione del valore di un elemento nei dati JSON](assets/capture-3.gif)

In AEM Forms OSGi - dobbiamo creare un bundle OSGi personalizzato per impostare il valore dell’elemento dati JSON. Il bundle viene fornito come parte di questa esercitazione.

Utilizziamo Passaggio del processo nel flusso di lavoro di AEM. Associamo il bundle OSGi &quot;Set Value of Element in Json&quot; a questo passaggio del processo.

È necessario trasmettere due argomenti al bundle del valore impostato. Il primo argomento è il percorso dell’elemento di cui è necessario impostare il valore. Il secondo argomento è il valore che deve essere impostato.

Ad esempio, nella schermata precedente, stiamo impostando il valore dell&#39;elemento initialStep su &quot;N&quot;

afData.afUnboundData.data.initialStep,N

Nel nostro esempio, abbiamo un semplice modulo di richiesta Time Off (Ferie). L&#39;iniziatore del presente modulo indica il proprio nome e le date delle ferie. All’invio, questo modulo viene inviato al &quot;manager&quot; per la revisione. Quando il manager apre il modulo, i campi del primo pannello vengono disattivati. Questo perché abbiamo impostato il valore dell’elemento del passaggio iniziale nei dati JSON su N.

In base al valore dei campi del passaggio iniziale, viene visualizzato il pannello approvatore in cui il &quot;responsabile&quot; può approvare o rifiutare la richiesta.

Date un&#39;occhiata alle regole impostate su &quot;Passaggio iniziale&quot;. In base al valore del campo initialStep, vengono recuperati i dettagli dell’utente utilizzando il Modello dati modulo, vengono compilati i campi appropriati e vengono nascosti/disabilitati i pannelli appropriati.

Per distribuire le risorse sul sistema locale:

* [Scarica e distribuisci DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che ti consente di impostare i valori di un elemento nei dati JSON inviati.

* [Scarica ed estrai il contenuto del file zip](assets/set-value-jsondata.zip)
   * Puntare il browser a [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
      * Importa e installa SetValueOfElementInJSONDataWorkflow.zip.Questo pacchetto include il modello di flusso di lavoro di esempio e il modello dati del modulo associati al modulo.

* Puntare il browser a [Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su Crea. | Caricamento file
* Carica il file TimeOffRequestForm.zip
  **Questo modulo è stato creato con AEM Forms 6.4. Assicurati di utilizzare AEM Forms 6.4 o versione successiva**
* Apri il [modulo](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Compila le date di inizio e fine e invia il modulo.
* Vai a [&quot;Posta in arrivo&quot;](http://localhost:4502/aem/inbox)
* Aprire il modulo associato all&#39;attività.
* Osserva che i campi nel primo pannello sono disattivati.
* Osserva che ora è visibile il pannello per approvare o rifiutare la richiesta.

>[!NOTE]
>
>Poiché il modulo adattivo viene precompilato utilizzando il profilo utente, assicurati che l&#39;amministratore [fornisca informazioni sul profilo utente](http://localhost:4502/security/users.html). Assicurati almeno di aver impostato i valori dei campi Nome, Cognome ed E-mail.
>Puoi abilitare la registrazione di debug abilitando il logger per com.aemforms.setvalue.core.SetValueInJson [da qui](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>Il bundle OSGi per l’impostazione del valore degli elementi dati nei dati JSON attualmente supporta la possibilità di impostare un valore elemento alla volta. Se si desidera impostare più valori di elemento, sarà necessario utilizzare più volte il passaggio del processo.
>
>Assicurati che il percorso del file di dati nelle opzioni di invio del modulo adattivo sia impostato su &quot;Data.xml&quot;. Questo perché il codice nel passaggio del processo cerca un file denominato Data.xml nella cartella del payload.
