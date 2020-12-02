---
title: Impostazione del valore di Json Data Element in  AEM Forms Workflow
seo-title: Impostazione del valore di Json Data Element in  AEM Forms Workflow
description: Poiché un modulo adattivo viene indirizzato a diversi utenti in AEM flusso di lavoro, è necessario nascondere o disabilitare determinati campi o pannelli in base alla persona che sta revisionando il modulo. Per soddisfare questi casi di utilizzo, in genere viene impostato il valore di un campo nascosto. In base alle regole aziendali relative al valore di questo campo nascosto, è possibile creare dei pannelli o campi specifici per nasconderli o disattivarli.
seo-description: Poiché un modulo adattivo viene indirizzato a diversi utenti in AEM flusso di lavoro, è necessario nascondere o disabilitare determinati campi o pannelli in base alla persona che sta revisionando il modulo. Per soddisfare questi casi di utilizzo, in genere viene impostato il valore di un campo nascosto. In base alle regole aziendali relative al valore di questo campo nascosto, è possibile creare dei pannelli o campi specifici per nasconderli o disattivarli.
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
translation-type: tm+mt
source-git-commit: 233ad7184cb48098253a78c07a3913356ac9e774
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---


# Impostazione del valore di JSON Data Element in  AEM Forms Workflow {#setting-value-of-json-data-element-in-aem-forms-workflow}

Poiché un modulo adattivo viene indirizzato a diversi utenti in AEM flusso di lavoro, è necessario nascondere o disabilitare determinati campi o pannelli in base alla persona che sta revisionando il modulo. Per soddisfare questi casi di utilizzo, in genere viene impostato il valore di un campo nascosto. In base alle regole aziendali relative al valore di questo campo nascosto, è possibile creare dei pannelli o campi specifici per nasconderli o disattivarli.

![Impostazione del valore di un elemento nei dati json](assets/capture-3.gif)

In  AEM Forms OSGI- dovremo scrivere un bundle OSGi personalizzato per impostare il valore dell&#39;elemento di dati JSON. Il bundle viene fornito come parte di questa esercitazione.

Utilizziamo Procedura nel flusso di lavoro AEM. Associamo il bundle OSGi &quot;Set Value of Element in Json&quot; a questo passaggio del processo.

È necessario trasmettere due argomenti al bundle set value. Il primo argomento è il percorso dell&#39;elemento il cui valore deve essere impostato. Il secondo argomento è il valore da impostare.

Ad esempio, nella schermata precedente, stiamo impostando il valore dell’elemento inialStep su &quot;N&quot;

afData.afUnboundData.data.initialStep,N

Nel nostro esempio, abbiamo un semplice modulo di richiesta del tempo. L&#39;iniziatore di questo modulo compila il proprio nome e l&#39;ora delle date. Al momento dell&#39;invio, il modulo va al manager per la revisione. Quando il manager apre il modulo, i campi del primo pannello sono disabilitati. Questo perché abbiamo impostato su N il valore dell&#39;elemento step iniziale nei dati JSON.

In base al valore dei campi del passaggio iniziale, viene visualizzato il pannello approver, in cui il &quot;manager&quot; può approvare o rifiutare la richiesta.

Date un&#39;occhiata alle regole impostate su &quot;Fase iniziale&quot;. In base al valore del campo initialStep, è possibile recuperare i dettagli utente utilizzando il modello dati modulo, compilare i campi appropriati e nascondere o disabilitare i pannelli appropriati.

Per distribuire le risorse sul sistema locale:

* [Download e implementazione di DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scaricate e distribuite il bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Si tratta del pacchetto OSGI personalizzato che consente di impostare i valori di un elemento nei dati json inviati.

* [Scaricare ed estrarre il contenuto del file zip](assets/set-value-jsondata.zip)
   * Posizionare il browser su [gestore pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
      * Importa e installa SetValueOfElementInJSONDataWorkflow.zip. Questo pacchetto include il modello di flusso di lavoro di esempio e il modello di dati modulo associati al modulo.

* Posizionare il browser su [Forms e Documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fate clic su Crea | Caricamento file
* Tempo di caricamento del fileOffRequestForm.zip
   **Questo modulo è stato creato utilizzando  AEM Forms 6.4. Verificate di essere su  AEM Forms 6.4 o versione successiva**
* Aprire il [modulo](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Compilare le date di inizio e di fine e inviare il modulo.
* Vai a [&quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* Aprire il modulo associato all&#39;attività.
* I campi del primo pannello sono disattivati.
* Il pannello per approvare o rifiutare la richiesta è ora visibile.

>[!NOTE]
>
>Poiché il modulo adattivo viene precompilato utilizzando il profilo utente, assicurarsi che le informazioni relative al profilo utente [admin ](http://localhost:4502/security/users.html) siano &lt;a1/>. Come minimo, accertatevi di aver impostato i valori dei campi Nome, Cognome e E-mail.
>È possibile abilitare la registrazione di debug abilitando logger per com.aemforms.setvalue.core.SetValueInJson [da qui](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>Il bundle OSGi per impostare il valore degli elementi di dati in JSON Data attualmente supporta la possibilità di impostare un valore di elemento alla volta. Se desiderate impostare più valori di elemento, dovrete utilizzare più volte il passaggio del processo.
>
>Assicurarsi che il percorso del file di dati nelle opzioni di invio del modulo adattivo sia impostato su &quot;Data.xml&quot;. Questo perché il codice nella fase di processo cerca un file denominato Data.xml sotto la cartella payload.
