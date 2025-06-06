---
title: Panoramica sulla configurazione del modulo adattivo per attivare il flusso di lavoro di AEM
description: Configurare le opzioni di payload durante l’attivazione del flusso di lavoro AEM all’invio del modulo
feature: Workflow
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
duration: 573
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 1%

---

# Configurazione del modulo adattivo per attivare il flusso di lavoro di AEM

## Prerequisiti

Il modulo di esempio utilizzato in questo flusso di lavoro è basato su un modello di modulo adattivo personalizzato che deve essere importato nel server AEM. Il modulo di esempio fornito deve essere importato dopo l’importazione del modello.

### Ottenere i modelli di modulo adattivo

* Scarica [Modello modulo adattivo](assets/af-form-template.zip)
* [Importa il modello utilizzando Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Caricare e installare il modello di modulo adattivo

### Ottieni il modulo adattivo di esempio

* Scarica [Modulo adattivo](assets/peak-application-form.zip)
* Passa a [Modulo e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su Crea -> Caricamento file
* Il modulo adattivo di esempio si trova in una cartella denominata [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

Il video seguente spiega come configurare un modulo adattivo per attivare un flusso di lavoro AEM
>[!VIDEO](https://video.tv.adobe.com/v/329443?quality=12&learn=on&captions=ita)

Il video seguente mostra il payload del flusso di lavoro e altri dettagli nell’archivio crx

>[!VIDEO](https://video.tv.adobe.com/v/329451?quality=12&learn=on&captions=ita)
