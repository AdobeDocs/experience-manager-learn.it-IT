---
title: ' AEM Forms con Marketo(Parte 1)'
seo-title: ' AEM Forms con Marketo(Parte 1)'
description: Esercitazione per integrare  AEM Forms con Marketo mediante  AEM Forms Form Data Model.
seo-description: Esercitazione per integrare  AEM Forms con Marketo mediante  AEM Forms Form Data Model.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---


#  AEM Forms Con Marketo

Marketo, parte del  Adobe, fornisce il software Marketing Automation incentrato sul marketing basato su account, tra cui e-mail, mobile, social, annunci digitali, gestione Web e analisi.

Utilizzando il modello dati modulo di  AEM Forms, ora è possibile integrare AEM Form con Marketo senza problemi.

[Ulteriori informazioni sul modello dati modulo](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo espone un&#39;API REST che consente l&#39;esecuzione remota di molte delle funzionalità del sistema. Dalla creazione di programmi all&#39;importazione in massa di lead, ci sono molte opzioni che consentono il controllo granulato fine di un&#39;istanza Marketo. Utilizzando il form Data Model è abbastanza semplice integrare  AEM Forms con Marketo.

Questa esercitazione illustra i passaggi necessari per integrare  AEM Forms con Marketo mediante l&#39;uso di Form Data Model. Al termine dell&#39;esercitazione, avrai a disposizione un bundle OSGi che eseguirà l&#39;autenticazione personalizzata rispetto a Marketo. Avrai anche configurato l&#39;origine dati utilizzando il file swagger fornito.

Per iniziare, si consiglia di avere familiarità con i seguenti argomenti elencati nella sezione Prerequisiti.

## Prerequisito

1. [Server AEM con  AEM Forms Add on installato](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ambiente di sviluppo AEM locale
1. Familiare con il modello dati modulo
1. Conoscenza di base dei file Swagger
1. Creazione di Forms adattivo

**ID segreto cliente e chiave segreta client**

Il primo passo nell&#39;integrazione di Marketo con  AEM Forms è quello di ottenere le credenziali API necessarie per effettuare le chiamate REST utilizzando l&#39;API. Sarà necessario disporre dei seguenti

1. client_id
1. client_secret
1. identity_endpoint
1. URL autenticazione.

[Seguire la documentazione ufficiale di Marketo per ottenere le proprietà di cui sopra.](https://developers.marketo.com/rest-api/) In alternativa, puoi anche contattare l’amministratore dell’istanza di Marketo.

**Prima di iniziare**

[Scaricate e decomprimete il file ZIP delle risorse correlate a questo articolo.](assets/aemformsandmarketo.zip) Il file zip contiene quanto segue:

1. BlankTemplatePackage.zip: modello di modulo adattivo. Importa questo utilizzando il gestore pacchetti.
1. marketo.json - Questo è il file swagger che verrà utilizzato per configurare l&#39;origine dati.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Questo è il bundle che esegue l&#39;autenticazione personalizzata. Sentitevi liberi di utilizzarlo se non riuscite a completare l&#39;esercitazione o se il bundle non funziona come previsto.
