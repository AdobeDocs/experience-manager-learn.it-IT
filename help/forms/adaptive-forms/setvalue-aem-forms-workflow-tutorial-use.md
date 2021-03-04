---
title: Utilizzo di setvalue nel flusso di lavoro di AEM Forms
seo-title: Utilizzo di setvalue nel flusso di lavoro di AEM Forms
description: Impostare il valore dell’elemento nei moduli adattivi inviati dati in AEM Forms OSGI
seo-description: Impostare il valore dell’elemento nei moduli adattivi inviati dati in AEM Forms OSGI
uuid: fe431e48-f05b-4b23-94d2-95d34d863984
feature: moduli adattivi, flusso di lavoro
topics: developing
audience: implementer
doc-type: article
activity: setup
discoiquuid: dbd87302-f770-4e61-b5ad-3fc5831b4613
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---


# Utilizzo di setvalue nel flusso di lavoro di AEM Forms

Imposta il valore di un elemento XML nei moduli adattivi inviati dati nel flusso di lavoro OSGI di AEM Forms.

![SetValue](assets/setvalue.png)

LiveCycle utilizzava un componente di valore impostato che consentiva di impostare un valore di un elemento XML.

In base a questo valore, quando il modulo è compilato con l’XML è possibile nascondere o disattivare alcuni campi o pannelli del modulo.

In AEM Forms OSGI- dovremo scrivere un bundle OSGi personalizzato per impostare il valore nel XML. Il bundle viene fornito come parte di questa esercitazione.
Usiamo Process Step nel flusso di lavoro AEM. Associamo il bundle OSGi &quot;Set Value of Element in XML&quot; a questo passaggio del processo.
Dobbiamo passare due argomenti al bundle di valori impostato. Il primo argomento è l&#39;XPath dell&#39;elemento XML il cui valore deve essere impostato. Il secondo argomento è il valore da impostare.
Ad esempio, nella schermata precedente, stiamo impostando il valore dell’elemento step iniziale su &quot;N&quot;.
In base a questo valore, alcuni pannelli nei Moduli adattivi vengono nascosti o visualizzati.
Nel nostro esempio, abbiamo un semplice Modulo di richiesta Time Off. L’iniziatore del modulo compila il nome e l’ora del modulo. Al momento dell’invio, questo modulo viene inviato ad &quot;admin&quot; per la revisione. Quando l’amministratore apre il modulo, i campi nel primo pannello sono disabilitati. Questo perché abbiamo impostato il valore dell&#39;elemento del passaggio iniziale nell&#39;XML su &quot;N&quot;.

In base al valore dei campi del passaggio iniziale, mostriamo il secondo pannello in cui l’&quot;amministratore&quot; può approvare o rifiutare la richiesta

Dai un&#39;occhiata alle regole impostate nel campo &quot;Time Off Requested by&quot; utilizzando l&#39;editor di regole.

Per distribuire le risorse sul sistema locale, segui i passaggi seguenti:

* [Distribuzione del bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Distribuisci il bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) di esempio. Questo è il bundle OSGI personalizzato che ti consente di impostare i valori di un elemento nei dati xml inviati

* [Scaricare ed estrarre il contenuto del file zip](assets/setvalueassets.zip)
* Posiziona il browser su [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa e installa il setValueWorkflow.zip. Questo è il modello di flusso di lavoro di esempio.
* Posiziona il browser su [Moduli e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su Crea | Caricamento file
* Carica TimeOfRequestForm.zip
* Apri [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Compila i 3 campi richiesti e invia
* Accedi come amministratore in AEM (se non lo hai già fatto)
* Vai a [&quot;AEM Inbox&quot;](http://localhost:4502/aem/inbox)
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
