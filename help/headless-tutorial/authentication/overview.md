---
title: Autenticazione per AEM as a Cloud Service da un’applicazione esterna
description: Scopri in che modo un’applicazione esterna può autenticare e interagire a livello di programmazione con AEM as a Cloud Service tramite HTTP utilizzando Token di accesso per lo sviluppo locale e Credenziali di servizio.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 0%

---

# Autenticazione basata su token per AEM as a Cloud Service

AEM espone una varietà di endpoint HTTP che possono interagire con modalità headless, da GraphQL, AEM Content Services all’API HTTP di Assets. Spesso, questi consumatori headless devono autenticarsi all’AEM per accedere a contenuti o azioni protetti. Per facilitare questa fase, l’AEM supporta l’autenticazione basata su token delle richieste HTTP provenienti da applicazioni, servizi o sistemi esterni.

In questo tutorial, esplora come un’applicazione esterna può autenticare e interagire a livello di programmazione con AEM as a Cloud Service tramite HTTP utilizzando i token di accesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Prerequisiti

Prima di seguire questa esercitazione, assicurati che siano presenti i seguenti elementi:

1. Accesso a un ambiente AEM as a Cloud Service (preferibilmente un ambiente di sviluppo o un programma sandbox)
1. Appartenenza al profilo di prodotto Amministratore AEM per i servizi di authoring dell’ambiente AEM as a Cloud Service
1. Iscrizione o accesso all&#39;amministratore dell&#39;organizzazione Adobe IMS (sarà necessario eseguire un&#39;inizializzazione una tantum delle [credenziali del servizio](./service-credentials.md))
1. L&#39;ultimo [sito WKND](https://github.com/adobe/aem-guides-wknd) distribuito nell&#39;ambiente di Cloud Service

## Panoramica dell’applicazione esterna

Questa esercitazione utilizza un&#39;[applicazione Node.js semplice](./assets/aem-guides_token-authentication-external-application.zip) eseguita dalla riga di comando per aggiornare i metadati delle risorse in AEM as a Cloud Service utilizzando [API HTTP Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

Il flusso di esecuzione dell’applicazione Node.js è il seguente:

![Applicazione esterna](./assets/overview/external-application.png)

1. L’applicazione Node.js viene richiamata dalla riga di comando
1. I parametri della riga di comando definiscono:
   + Host del servizio AEM as a Cloud Service Author a cui connettersi (`aem`)
   + Cartella di risorse AEM di cui vengono aggiornate le risorse (`folder`)
   + Proprietà e valore dei metadati da aggiornare (`propertyName` e `propertyValue`)
   + Percorso locale del file che fornisce le credenziali necessarie per accedere ad AEM as a Cloud Service (`file`)
1. Il token di accesso utilizzato per l&#39;autenticazione a AEM deriva dal file JSON fornito tramite il parametro della riga di comando `file`

   a. Se nel file JSON (`file`) vengono fornite le credenziali del servizio utilizzate per lo sviluppo non locale, il token di accesso viene recuperato dalle API Adobe IMS
1. L&#39;applicazione utilizza il token di accesso per accedere a AEM ed elencare tutte le risorse nella cartella specificata nel parametro della riga di comando `folder`
1. Per ogni risorsa della cartella, l&#39;applicazione aggiorna i metadati in base al nome e al valore della proprietà specificati nei parametri della riga di comando `propertyName` e `propertyValue`

Anche se questa applicazione di esempio è Node.js, queste interazioni possono essere sviluppate utilizzando linguaggi di programmazione diversi ed eseguite da altri sistemi esterni.

## Token di accesso per lo sviluppo locale

I token di accesso per lo sviluppo locale vengono generati per un ambiente AEM as a Cloud Service specifico e forniscono accesso ai servizi Author e Publish.  Questi token di accesso sono temporanei e devono essere utilizzati solo durante lo sviluppo di applicazioni o sistemi esterni che interagiscono con l’AEM tramite HTTP. Invece di dover ottenere e gestire le credenziali del servizio bonafide, uno sviluppatore può generare in modo rapido e semplice un token di accesso temporaneo che consente di sviluppare l’integrazione.

+ [Come utilizzare il token di accesso per lo sviluppo locale](./local-development-access-token.md)

## Credenziali servizio

Le credenziali di servizio sono credenziali bonafide utilizzate in qualsiasi scenario non di sviluppo, ovviamente di produzione, che facilitano l’autenticazione e l’interazione di un’applicazione o un sistema esterno con AEM as a Cloud Service su HTTP. Le credenziali del servizio non vengono inviate a AEM per l&#39;autenticazione, ma l&#39;applicazione esterna le utilizza per generare un JWT, che viene scambiato con le API di Adobe IMS _per_ un token di accesso, che può quindi essere utilizzato per autenticare le richieste HTTP ad AEM as a Cloud Service.

+ [Come utilizzare le credenziali del servizio](./service-credentials.md)

## Risorse aggiuntive

+ [Scarica l’applicazione di esempio](./assets/aem-guides_token-authentication-external-application.zip)
+ Altri esempi di codice per la creazione e lo scambio di JWT
   + [Esempi di codici Node.js, Java, Python, C#.NET e PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [Esempio di codice basato su JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
