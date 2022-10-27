---
title: Utilizzo di setvalue nel flusso di lavoro di AEM Forms
description: Impostare il valore dell'elemento in Adattivo Forms dati inviati in AEM Forms OSGI
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# Utilizzo di setvalue nel flusso di lavoro di AEM Forms

Imposta il valore di un elemento XML in Adattivo Forms inviato i dati nel flusso di lavoro AEM Forms OSGI.

![SetValue](assets/setvalue.png)

LiveCycle utilizzato per avere un componente valore impostato che consente di impostare il valore di un elemento XML.

In base a questo valore, quando il modulo è compilato con l’XML è possibile nascondere o disattivare alcuni campi o pannelli del modulo.

In AEM Forms OSGI- dovremo scrivere un bundle OSGi personalizzato per impostare il valore nel XML. Il bundle viene fornito come parte di questa esercitazione.
Usiamo Process Step nel flusso di lavoro AEM. Associamo il bundle OSGi &quot;Set Value of Element in XML&quot; a questo passaggio del processo.
Dobbiamo passare due argomenti al bundle di valori impostato. Il primo argomento è l&#39;XPath dell&#39;elemento XML il cui valore deve essere impostato. Il secondo argomento è il valore da impostare.
Ad esempio, nella schermata precedente, stiamo impostando il valore dell’elemento step iniziale su &quot;N&quot;.
In base a questo valore, alcuni pannelli di Adaptive Forms sono nascosti o visualizzati.
Nel nostro esempio, abbiamo un semplice Modulo di richiesta Time Off. L’iniziatore del modulo compila il nome e l’ora del modulo. Al momento dell’invio, questo modulo viene inviato ad &quot;admin&quot; per la revisione. Quando l’amministratore apre il modulo, i campi nel primo pannello sono disabilitati. Questo perché abbiamo impostato il valore dell&#39;elemento del passaggio iniziale nell&#39;XML su &quot;N&quot;.

In base al valore dei campi del passaggio iniziale, mostriamo il secondo pannello in cui l’&quot;amministratore&quot; può approvare o rifiutare la richiesta

Dai un&#39;occhiata alle regole impostate nel campo &quot;Time Off Requested by&quot; utilizzando l&#39;editor di regole.

Per distribuire le risorse sul sistema locale, segui i passaggi seguenti:

* [Distribuzione del bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Distribuire il bundle di esempio](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che ti consente di impostare i valori di un elemento nei dati xml inviati

* [Scaricare ed estrarre il contenuto del file zip](assets/setvalueassets.zip)
* Posiziona il browser su [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa e installa il setValueWorkflow.zip. Questo è il modello di flusso di lavoro di esempio.
* Posiziona il browser su [Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su Crea | Caricamento file
* Carica TimeOfRequestForm.zip
* Apri [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Compila i 3 campi richiesti e invia
* Accedi come amministratore in a AEM (se non lo hai già fatto)
* Vai a [&quot;Casella in entrata AEM&quot;](http://localhost:4502/aem/inbox)
* Apri il modulo &quot;Review Time Off Request&quot; (Ora di revisione della richiesta)
* I campi nel primo pannello sono disabilitati. Questo perché il modulo viene aperto dal revisore. Inoltre, il pannello per approvare o rifiutare la richiesta è ora visibile

>[!NOTE]
>
>È possibile abilitare la registrazione di debug abilitando logger per
>com.aemforms.setvalue.core.SetValueinXml
>indicando il browser a http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Assicurati che il percorso del file dati nelle opzioni di invio del modulo adattivo sia impostato su &quot;Data.xml&quot;. Questo perché il passaggio del processo cerca un file denominato Data.xml sotto la cartella payload
