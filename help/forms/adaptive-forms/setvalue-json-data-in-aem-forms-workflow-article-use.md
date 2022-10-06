---
title: Impostazione del valore di Json Data Element nel flusso di lavoro AEM Forms
description: Poiché un modulo adattivo viene indirizzato a diversi utenti in AEM flusso di lavoro, è necessario nascondere o disattivare alcuni campi o pannelli in base alla persona che sta esaminando il modulo. Per soddisfare questi casi d’uso, in genere viene impostato un valore di un campo nascosto. In base al valore delle regole business di questo campo nascosto può essere creato per nascondere/disabilitare pannelli o campi appropriati.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 1%

---

# Impostazione del valore di JSON Data Element nel flusso di lavoro AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Poiché un modulo adattivo viene indirizzato a diversi utenti in AEM flusso di lavoro, è necessario nascondere o disattivare alcuni campi o pannelli in base alla persona che sta esaminando il modulo. Per soddisfare questi casi d’uso, in genere viene impostato un valore di un campo nascosto. In base al valore delle regole business di questo campo nascosto può essere creato per nascondere/disabilitare pannelli o campi appropriati.

![Impostazione del valore di un elemento nei dati json](assets/capture-3.gif)

In AEM Forms OSGi - dobbiamo creare un bundle OSGi personalizzato per impostare il valore dell&#39;elemento dati JSON. Il bundle viene fornito come parte di questa esercitazione.

Usiamo Process Step nel flusso di lavoro AEM. Associamo il bundle OSGi &quot;Set Value of Element in Json&quot; a questo passaggio del processo.

Dobbiamo passare due argomenti al bundle di valori impostato. Il primo argomento è il percorso dell’elemento il cui valore deve essere impostato. Il secondo argomento è il valore da impostare.

Ad esempio, nella schermata precedente, stiamo impostando il valore dell’elemento inialStep su &quot;N&quot;

afData.afUnboundData.data.initialStep,N

Nel nostro esempio, abbiamo un semplice Modulo di richiesta Time Off. L’iniziatore del modulo compila il nome e l’ora del modulo. Al momento dell&#39;invio, questo modulo va al &quot;responsabile&quot; per la revisione. Quando il gestore apre il modulo, i campi del primo pannello vengono disabilitati. Questo perché abbiamo impostato il valore dell’elemento del passaggio iniziale nei dati JSON su N.

In base al valore dei campi del passaggio iniziale, viene visualizzato il pannello approvatore in cui il &quot;responsabile&quot; può approvare o rifiutare la richiesta.

Dai un&#39;occhiata alle regole impostate su &quot;Passaggio iniziale&quot;. In base al valore del campo initialStep , recuperiamo i dettagli utente utilizzando Form Data Model e compiliamo i campi appropriati, quindi nascondiamo/disattiviamo i pannelli appropriati.

Per distribuire le risorse sul sistema locale:

* [Scarica e distribuisci DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che ti consente di impostare i valori di un elemento nei dati json inviati.

* [Scaricare ed estrarre il contenuto del file zip](assets/set-value-jsondata.zip)
   * Posiziona il browser su [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
      * Importa e installa il setValueOfElementInJSONDataWorkflow.zip.Questo pacchetto presenta il modello di flusso di lavoro di esempio e il modello dati modulo associati al modulo.

* Posiziona il browser su [Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su Crea | Caricamento file
* Carica il file TimeOffRequestForm.zip
   **Questo modulo è stato creato con AEM Forms 6.4. Assicurati di essere su AEM Forms 6.4 o versione successiva**
* Apri [modulo](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Compila le date di inizio e di fine e invia il modulo.
* Vai a [&quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* Aprire il modulo associato all&#39;attività.
* I campi nel primo pannello sono disabilitati.
* Il pannello per approvare o rifiutare la richiesta è ora visibile.

>[!NOTE]
>
>Poiché il modulo adattivo viene precompilato utilizzando il profilo utente, assicurati che l’amministratore [informazioni sul profilo utente ](http://localhost:4502/security/users.html). Assicurati almeno di aver impostato i valori dei campi Nome, Cognome e Email .
>Puoi abilitare la registrazione di debug abilitando il logger per com.aemforms.setvalue.core.SetValueInJson [da qui](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>Il bundle OSGi per l&#39;impostazione del valore degli elementi dati in JSON Data attualmente supporta la possibilità di impostare un valore di elemento alla volta. Se desideri impostare più valori di elemento, dovrai utilizzare più volte il passaggio del processo.
>
>Assicurati che il percorso del file dati nelle opzioni di invio del modulo adattivo sia impostato su &quot;Data.xml&quot;. Questo perché il codice nel passaggio del processo cerca un file denominato Data.xml sotto la cartella payload.
