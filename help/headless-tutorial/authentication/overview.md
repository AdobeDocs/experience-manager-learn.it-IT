---
title: Autenticazione per AEM come Cloud Service da un’applicazione esterna
description: Scopri in che modo un’applicazione esterna può autenticare e interagire in modo programmatico con AEM come Cloud Service tramite HTTP utilizzando i token di accesso allo sviluppo locale e le credenziali del servizio.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 0%

---

# Autenticazione basata su token per AEM come Cloud Service

AEM espone una varietà di endpoint HTTP con cui è possibile interagire in modo headless, da GraphQL, AEM Content Services all’API HTTP Assets. Spesso, questi consumatori headless possono aver bisogno di autenticarsi per AEM per accedere a contenuti o azioni protetti. Per facilitare questa fase, AEM supporta l’autenticazione basata su token delle richieste HTTP provenienti da applicazioni, servizi o sistemi esterni.

In questa esercitazione approfondisci come un’applicazione esterna può autenticarsi e interagire in modo programmatico con per AEM come Cloud Service tramite HTTP utilizzando i token di accesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Prerequisiti

Assicurati che quanto segue sia già presente prima di seguire questa esercitazione:

1. Accesso a am AEM come ambiente di Cloud Service (preferibilmente un ambiente di sviluppo o un programma sandbox)
1. Iscrizione al AEM come ambiente di Cloud Service Author services AEM profilo di prodotto Administrator
1. Iscrizione o accesso all’amministratore dell’organizzazione Adobe IMS (dovrà eseguire un’inizializzazione una tantum delle [Credenziali del servizio](./service-credentials.md))
1. Ultimo [sito WKND](https://github.com/adobe/aem-guides-wknd) implementato nell&#39;ambiente di Cloud Service

## Panoramica dell’applicazione esterna

Questa esercitazione utilizza una [semplice applicazione Node.js](./assets/aem-guides_token-authentication-external-application.zip) eseguita dalla riga di comando per aggiornare i metadati delle risorse su AEM come Cloud Service utilizzando [API HTTP Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

Il flusso di esecuzione dell&#39;applicazione Node.js è il seguente:

![Applicazione esterna](./assets/overview/external-application.png)

1. L&#39;applicazione Node.js viene richiamata dalla riga di comando
1. I parametri della riga di comando definiscono:
   + Il AEM come host del servizio Author di Cloud Service a cui connettersi (`aem`)
   + La cartella di risorse AEM le cui risorse verranno aggiornate (`folder`)
   + Proprietà e valore dei metadati da aggiornare (`propertyName` e `propertyValue`)
   + Percorso locale del file che fornisce le credenziali necessarie per accedere a AEM come Cloud Service (`file`)
1. Il token di accesso utilizzato per l’autenticazione a AEM deriva dal file JSON fornito tramite il parametro della riga di comando `file`

   a) Se le credenziali del servizio utilizzate per lo sviluppo non locale sono fornite nel file JSON (`file`), il token di accesso viene recuperato dalle API Adobe IMS
1. L’applicazione utilizza il token di accesso per accedere AEM ed elencare tutte le risorse nella cartella specificata nel parametro della riga di comando `folder`
1. Per ogni risorsa della cartella, l&#39;applicazione aggiorna i metadati in base al nome e al valore della proprietà specificati nei parametri della riga di comando `propertyName` e `propertyValue`

Anche se questa applicazione di esempio è Node.js, queste interazioni possono essere sviluppate utilizzando diversi linguaggi di programmazione ed eseguite da altri sistemi esterni.

## Token di accesso per lo sviluppo locale

I token di accesso allo sviluppo locale vengono generati per un AEM specifico come ambiente di Cloud Service e forniscono accesso ai servizi Author e Publish.  Questi token di accesso sono temporanei e devono essere utilizzati solo durante lo sviluppo di applicazioni o sistemi esterni che interagiscono con AEM tramite HTTP. Invece di uno sviluppatore che deve ottenere e gestire le credenziali di servizio del bonafide, possono generare rapidamente e facilmente un token di accesso temporaneo che consenta loro di sviluppare la loro integrazione.

+ [Come utilizzare il token di accesso allo sviluppo locale](./local-development-access-token.md)

## Credenziali del servizio

Le credenziali del servizio sono le credenziali del bonafide utilizzate in qualsiasi scenario non di sviluppo - ovviamente la produzione - che facilitano la capacità di un&#39;applicazione esterna o di un sistema di autenticarsi e interagire con AEM come Cloud Service su HTTP. Le credenziali del servizio non vengono inviate a AEM per l’autenticazione, ma l’applicazione esterna le utilizza per generare un JWT, che viene scambiato con le API di Adobe IMS _per_ un token di accesso, che può quindi essere utilizzato per autenticare le richieste HTTP per AEM come Cloud Service.

+ [Come utilizzare le credenziali del servizio](./service-credentials.md)

## Altro materiale di riferimento

+ [Scarica l’applicazione di esempio](./assets/aem-guides_token-authentication-external-application.zip)
+ Altri esempi di codice per la creazione e lo scambio di JWT
   + [Esempi di codice Node.js, Java, Python, C#.NET e PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Esempio di codice basato su JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
