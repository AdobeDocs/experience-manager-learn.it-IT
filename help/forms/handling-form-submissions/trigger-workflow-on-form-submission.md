---
title: Attiva AEM flusso di lavoro all’invio del modulo
description: Configurare il modulo adattivo per attivare AEM flusso di lavoro.
sub-product: forms
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: kt-5407.jpg
translation-type: tm+mt
source-git-commit: 738e356c4e72e0c3518bb5fd4ad6a076522e9f5c
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---


# Configurazione del modulo adattivo per attivare AEM flusso di lavoro

* Download del modulo [adattivo](assets/time-off-application.zip)
* Passa a [moduli e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fate clic su Crea > Caricamento file. Selezionare il modulo scaricato al punto 1
* Aprire il modulo [adattivo in modalità](http://localhost:4502/editor.html/content/forms/af/timeofapplication.html)di modifica.
* Aprire Content Explorer
   ![Content Explorer](assets/af-workflow-submission.PNG)
* Selezionare il nodo Contenitore modulo e aprire le relative proprietà di configurazione
   ![Invio](assets/af-workflow-submission1.PNG)
* Espandi il pannello Invia
* Impostare l&#39;azione di invio del modulo come specificato nella schermata precedente.
   _Assicurarsi di prendere nota del valore specificato nel campo Percorso file dati. Questo valore deve corrispondere al valore specificato nella sezione precompilata del componente Assegna attività del flusso di lavoro._

Ora, quando si compila e si invia il modulo adattivo, viene attivato il flusso di lavoro associato all&#39;azione di invio del modulo.
