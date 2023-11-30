---
title: Panoramica sulla configurazione del modulo adattivo per attivare il flusso di lavoro AEM
description: Configurare le opzioni di payload quando si attiva il flusso di lavoro AEM all’invio del modulo
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
jira: KT-5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 3%

---

# Configurazione del modulo adattivo per attivare il flusso di lavoro AEM

## Prerequisiti

Il modulo di esempio utilizzato in questo flusso di lavoro è basato su un modello di modulo adattivo personalizzato che deve essere importato nel server AEM. Il modulo di esempio fornito deve essere importato dopo l’importazione del modello.

### Ottenere i modelli di modulo adattivo

* Scarica [Modello modulo adattivo](assets/af-form-template.zip)
* [Importare il modello utilizzando Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Caricare e installare il modello di modulo adattivo

### Ottieni il modulo adattivo di esempio

* Scarica [Modulo adattivo](assets/peak-application-form.zip)
* Sfoglia per [Modulo E Documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su Crea -> Caricamento file
* Il modulo adattivo di esempio viene inserito in una cartella denominata [Forms applicazione](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

Il video seguente spiega come configurare un modulo adattivo per attivare un flusso di lavoro AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

Il video seguente mostra il payload del flusso di lavoro e altri dettagli nell’archivio crx

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
