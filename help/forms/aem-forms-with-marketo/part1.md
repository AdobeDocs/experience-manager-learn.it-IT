---
title: AEM Forms con Marketo(Parte 1)
seo-title: AEM Forms con Marketo(Parte 1)
description: Esercitazione per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
seo-description: Esercitazione per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: '"Moduli adattivi, modello dati modulo"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---


# AEM Forms Con Marketo

Marketo, parte di Adobe, fornisce software di automazione marketing incentrato sul marketing basato su account, tra cui e-mail, dispositivi mobili, social, annunci digitali, gestione web e analisi.

Utilizzando il modello dati modulo di AEM Forms ora è possibile integrare AEM Form con Marketo senza soluzione di continuità.

[Ulteriori informazioni sul modello dati modulo](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo espone un’API REST che consente l’esecuzione remota di molte delle funzionalità del sistema. Dalla creazione di programmi all’importazione in serie di lead, sono disponibili numerose opzioni che consentono il controllo a grana fine di un’istanza Marketo. Utilizzando il modello dati modulo è abbastanza semplice integrare AEM Forms con Marketo.

Questa esercitazione illustra i passaggi necessari per integrare AEM Forms con Marketo utilizzando il modello dati modulo. Al termine dell&#39;esercitazione avrai a disposizione un bundle OSGi che eseguirà l&#39;autenticazione personalizzata contro Marketo. Avrai anche configurato l’origine dati utilizzando il file swagger fornito.

Per iniziare, è consigliabile avere familiarità con i seguenti argomenti elencati nella sezione Prerequisito .

## Prerequisito

1. [Server AEM con pacchetto AEM Forms Add on installato](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ambiente di sviluppo AEM locale
1. Familiare con il modello dati del modulo
1. Conoscenza di base dei file Swagger
1. Creazione di moduli adattivi

**ID segreto client e chiave segreta client**

Il primo passaggio nell’integrazione di Marketo con AEM Forms è quello di ottenere le credenziali API necessarie per effettuare le chiamate REST utilizzando l’API. Sarà necessario quanto segue

1. client_id
1. client_secret
1. identity_endpoint
1. URL di autenticazione.

[Segui la documentazione ufficiale di Marketo per ottenere le proprietà di cui sopra.](https://developers.marketo.com/rest-api/) In alternativa, puoi anche contattare l’amministratore dell’istanza Marketo.

**Prima di iniziare**

[Scarica e decomprimi le risorse correlate a questo articolo.](assets/aemformsandmarketo.zip) Il file zip contiene quanto segue:

1. BlankTemplatePackage.zip - Questo è il modello di modulo adattivo. Importa questo file utilizzando il gestore dei pacchetti.
1. marketo.json - Questo è il file swagger che verrà utilizzato per configurare l&#39;origine dati.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Questo è il bundle che esegue l&#39;autenticazione personalizzata. Puoi utilizzarlo gratuitamente se non riesci a completare l’esercitazione o se il pacchetto non funziona come previsto.
