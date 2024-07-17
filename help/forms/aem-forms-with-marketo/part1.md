---
title: Integrare AEM Forms e Marketo
description: Scopri come integrare AEM Forms e Marketo utilizzando AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 77
source-git-commit: 8bde459ae9a6e261cfc3aff308babe9de6e56059
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 1%

---

# Integrare AEM Forms e Marketo

Marketo, parte di Adobe fornisce software di automazione del marketing incentrato sul marketing basato sull’account, tra cui e-mail, dispositivi mobili, social, annunci digitali, gestione web e analisi.

Utilizzando il modello dati del modulo di AEM Forms, ora possiamo integrare facilmente il modulo AEM con Marketo.

[Ulteriori informazioni sul modello dati modulo](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo espone un’API REST che consente l’esecuzione remota di molte delle funzionalità del sistema. Dalla creazione di programmi all’importazione in blocco di lead, sono disponibili molte opzioni che consentono il controllo dettagliato di un’istanza di Marketo. Utilizzando il modello dati modulo è abbastanza semplice integrare AEM Forms con Marketo.

Questo tutorial illustra i passaggi necessari per l’integrazione di AEM Forms con Marketo utilizzando il modello dati del modulo. Al termine dell’esercitazione avrai a disposizione un bundle OSGi che eseguirà l’autenticazione personalizzata in base a Marketo. Avrai anche configurato l’origine dati utilizzando il file swagger fornito.

Per iniziare, ti consigliamo vivamente di conoscere i seguenti argomenti elencati nella sezione Prerequisiti.

## Prerequisito

1. [Server AEM con pacchetto AEM Forms Add on installato](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ambiente locale di sviluppo AEM
1. Familiarità con il modello dati del modulo
1. Conoscenza di base dei file Swagger
1. Creazione di un Forms adattivo

**ID segreto client e chiave segreto client**

Il primo passaggio nell’integrazione di Marketo con AEM Forms consiste nell’ottenere le credenziali API necessarie per effettuare le chiamate REST utilizzando l’API. Sono necessari i seguenti elementi

1. client_id
1. client_secret
1. identity_endpoint
1. URL di autenticazione

[Segui la documentazione ufficiale di Marketo per ottenere le proprietà di cui sopra.](https://developers.marketo.com/rest-api/) In alternativa, puoi anche contattare l&#39;amministratore della tua istanza di Marketo.

**Prima di iniziare**

* [Scarica e decomprimi le risorse correlate a questa esercitazione](assets/marketo-integration-assets.zip)

Il file zip contiene quanto segue:

1. BlankTemplatePackage.zip - Questo è il modello di modulo adattivo. Importa questo utilizzando Gestione pacchetti.
1. marketo.json - Questo è il file swagger utilizzato per configurare l’origine dati.
1. Assicurati di modificare la proprietà host in marketo.json per puntare all’istanza marketo

## Passaggi successivi

[Creazione di Data Source](./part2.md)
