---
title: Utilizzo di setvalue nel flusso di lavoro di AEM Forms
description: Impostare il valore dell’elemento nei dati inviati da Forms adattivo in AEM Forms OSGI
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 105
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# Utilizzo di setvalue nel flusso di lavoro di AEM Forms

Imposta il valore di un elemento XML nei dati inviati da Adaptive Forms nel flusso di lavoro OSGI di AEM Forms.

![ImpostaValore](assets/setvalue.png)

LiveCycle utilizzato per impostare un componente valore che consente di impostare il valore di un elemento XML.

In base a questo valore, quando il modulo viene compilato con il codice XML è possibile nascondere o disabilitare alcuni campi o pannelli del modulo.

In AEM Forms OSGI- dovremo scrivere un bundle OSGi personalizzato per impostare il valore nel codice XML. Il bundle viene fornito come parte di questa esercitazione.
Utilizziamo Process Step nel flusso di lavoro dell’AEM. Associamo il bundle OSGi &quot;Set Value of Element in XML&quot; a questo passaggio del processo.
È necessario trasmettere due argomenti al bundle del valore impostato. Il primo argomento è l&#39;XPath dell&#39;elemento XML di cui è necessario impostare il valore. Il secondo argomento è il valore che deve essere impostato.
Ad esempio, nella schermata precedente, stiamo impostando il valore dell’elemento iniziale su &quot;N&quot;.
In base a questo valore, alcuni pannelli nel Forms adattivo sono nascosti o visualizzati.
Nel nostro esempio, abbiamo un semplice modulo di richiesta Time Off (Ferie). L&#39;iniziatore del presente modulo indica il proprio nome e le date delle ferie. All’invio, questo modulo viene inviato all’amministratore per la revisione. Quando l’amministratore apre il modulo, i campi nel primo pannello vengono disattivati. Questo perché abbiamo impostato il valore dell’elemento del passaggio iniziale nell’XML su &quot;N&quot;.

In base al valore dei campi del passaggio iniziale, mostriamo il secondo pannello in cui l’amministratore può approvare o rifiutare la richiesta

Osserva le regole impostate per il campo &quot;Time Off Requested by&quot; (Ferie richieste da) utilizzando l’editor di regole.

Per distribuire le risorse sul sistema locale, effettua le seguenti operazioni:

* [Distribuire il bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Distribuire il bundle di esempio](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che ti consente di impostare i valori di un elemento nei dati XML inviati

* [Scarica ed estrai il contenuto del file zip](assets/setvalueassets.zip)
* Puntare il browser a [gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa e installa setValueWorkflow.zip. Questo include il modello di flusso di lavoro di esempio.
* Puntare il browser a [Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su Crea. | Caricamento file
* Carica TimeOfRequestForm.zip
* Apri [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Compila i 3 campi obbligatori e invia
* Accedi come amministratore all’AEM (se non lo hai già fatto)
* Vai a [&quot;Casella in entrata AEM&quot;](http://localhost:4502/aem/inbox)
* Apri il modulo &quot;Review Time Off Request&quot; (Rivedi richiesta di ferie)
* Osserva che i campi nel primo pannello sono disattivati. Questo perché il modulo viene aperto dal revisore. Inoltre, ora è visibile il pannello per approvare o rifiutare la richiesta

>[!NOTE]
>
>È possibile abilitare la registrazione di debug abilitando il logger per
>com.aemforms.setvalue.core.SetValueinXml
>puntando il browser su http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Assicurati che il percorso del file di dati nelle opzioni di invio del modulo adattivo sia impostato su &quot;Data.xml&quot;. Questo perché il passaggio del processo cerca un file denominato Data.xml nella cartella del payload
