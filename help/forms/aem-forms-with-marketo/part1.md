---
title: AEM Forms con Marketo (parte 1)
description: Esercitazione per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# AEM Forms Con Marketo

Marketo, parte di Adobe, fornisce software di automazione marketing incentrato sul marketing basato su account, tra cui e-mail, mobile, social, annunci digitali, gestione web e analisi.

Utilizzando il modello dati modulo di AEM Forms ora è possibile integrare AEM modulo con Marketo senza soluzione di continuità.

[Ulteriori informazioni sul modello dati modulo](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo espone un’API REST che consente l’esecuzione remota di molte delle funzionalità del sistema. Dalla creazione di programmi all’importazione in blocco di lead, sono disponibili numerose opzioni che consentono il controllo a grana fine di un’istanza di Marketo. Utilizzando il modello dati modulo è abbastanza semplice integrare AEM Forms con Marketo.

Questa esercitazione illustra i passaggi necessari per integrare AEM Forms con Marketo utilizzando Form Data Model. Al termine dell&#39;esercitazione avrai a disposizione un bundle OSGi che eseguirà l&#39;autenticazione personalizzata rispetto a Marketo. Avrai anche configurato l’origine dati utilizzando il file swagger fornito.

Per iniziare, è consigliabile avere familiarità con i seguenti argomenti elencati nella sezione Prerequisito .

## Prerequisito

1. [AEM server con il pacchetto AEM Forms Add on installato](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ambiente di sviluppo AEM locale
1. Familiare con il modello dati del modulo
1. Conoscenza di base dei file Swagger
1. Creazione di Forms adattivo

**ID segreto client e chiave segreta client**

Il primo passaggio nell’integrazione di Marketo con AEM Forms è quello di ottenere le credenziali API necessarie per effettuare le chiamate REST utilizzando l’API. Sarà necessario quanto segue

1. client_id
1. client_secret
1. identity_endpoint
1. URL di autenticazione

[Segui la documentazione ufficiale di Marketo per ottenere le proprietà di cui sopra.](https://developers.marketo.com/rest-api/) In alternativa, puoi anche contattare l’amministratore della tua istanza Marketo.

**Prima di iniziare**

[Scarica e decomprimi le risorse correlate a questo articolo.](assets/aemformsandmarketo.zip) Il file zip contiene quanto segue:

1. BlankTemplatePackage.zip - Questo è il modello di modulo adattivo. Importa questo file utilizzando il gestore dei pacchetti.
1. marketo.json - Questo è il file swagger utilizzato per configurare l&#39;origine dati.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Questo è il bundle che esegue l&#39;autenticazione personalizzata. Puoi utilizzarlo gratuitamente se non riesci a completare l’esercitazione o se il pacchetto non funziona come previsto.

## Passaggi successivi

[Creare un’autenticazione personalizzata](./part2.md)
