---
title: Autenticazione in AEM as a Cloud Service da un’applicazione esterna
description: Scopri come un’applicazione esterna può autenticare e interagire a livello di programmazione con AEM as a Cloud Service tramite HTTP utilizzando un token di accesso per lo sviluppo locale e le credenziali di servizio.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: ht
source-wordcount: '621'
ht-degree: 100%

---

# Autenticazione basata su token per AEM as a Cloud Service

AEM espone diversi endpoint HTTP con cui si può interagire in modo headless, da GraphQL, AEM Content Services all’API HTTP di Assets. Spesso, questi consumatori headless potrebbero dover effettuare l’autenticazione ad AEM per accedere a contenuti o azioni protetti. A tal fine, AEM supporta l’autenticazione basata su token delle richieste HTTP provenienti da applicazioni, servizi o sistemi esterni.

In questo tutorial, esploreremo come un’applicazione esterna può autenticare e interagire a livello di programmazione con AEM as a Cloud Service tramite HTTP utilizzando i token di accesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Prerequisiti

Prima di seguire questo tutorial, assicurati di disporre di quanto segue:

1. Accesso a un ambiente AEM as a Cloud Service (preferibilmente un ambiente di sviluppo o un programma sandbox)
1. Iscrizione al profilo di prodotto Amministratore AEM per i servizi di authoring dell’ambiente AEM as a Cloud Service
1. Iscrizione o accesso all’amministratore dell’organizzazione Adobe IMS (sarà necessario eseguire una singola inizializzazione delle [credenziali del servizio](./service-credentials.md))
1. Il [sito WKND](https://github.com/adobe/aem-guides-wknd) più recente distribuito nell’ambiente Cloud Service

## Panoramica dell’applicazione esterna

Questo tutorial utilizza un’[applicazione Node.js semplice](./assets/aem-guides_token-authentication-external-application.zip) eseguita dalla riga di comando per aggiornare i metadati delle risorse in AEM as a Cloud Service utilizzando l’[API HTTP di Assets](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets).

Il flusso di esecuzione dell’applicazione Node.js è il seguente:

![Applicazione esterna](./assets/overview/external-application.png)

1. L’applicazione Node.js viene richiamata dalla riga di comando
1. I parametri della riga di comando definiscono:
   + L’host del servizio di authoring AEM as a Cloud Service a cui connettersi (`aem`)
   + La cartella di risorse AEM di cui vengono aggiornate le risorse (`folder`)
   + La proprietà e il valore dei metadati da aggiornare (`propertyName` e `propertyValue`)
   + Il percorso locale del file che fornisce le credenziali necessarie per accedere ad AEM as a Cloud Service (`file`)
1. Il token di accesso utilizzato per l’autenticazione in AEM deriva dal file JSON fornito tramite il parametro della riga di comando `file`

   a. Se nel file JSON (`file`) vengono fornite le credenziali del servizio utilizzate per lo sviluppo non locale, il token di accesso viene recuperato dalle API di Adobe IMS
1. L’applicazione utilizza il token di accesso per accedere ad AEM ed elencare tutte le risorse nella cartella specificata nel parametro della riga di comando `folder`
1. Per ciascuna risorsa della cartella, l’applicazione aggiorna i metadati in base al nome e al valore della proprietà specificati nei parametri della riga di comando `propertyName` e `propertyValue`

Anche se questa applicazione di esempio è Node.js, queste interazioni possono essere sviluppate utilizzando linguaggi di programmazione diversi ed eseguite da altri sistemi esterni.

## Token di accesso per lo sviluppo locale

I token di accesso per lo sviluppo locale vengono generati per un ambiente AEM as a Cloud Service specifico e forniscono accesso ai servizi di authoring e pubblicazione.  Questi token di accesso sono temporanei e devono essere utilizzati solo durante lo sviluppo di applicazioni o sistemi esterni che interagiscono con AEM tramite HTTP. Invece di dover ottenere e gestire le credenziali del servizio autentiche, uno sviluppatore può generare in modo rapido e semplice un token di accesso temporaneo che gli consente di sviluppare l’integrazione.

+ [Come utilizzare il token di accesso per lo sviluppo locale](./local-development-access-token.md)

## Credenziali del servizio

Le credenziali del servizio sono credenziali autentiche utilizzate in qualsiasi scenario non di sviluppo, la maggior parte ovviamente di produzione, che permettono a un’applicazione o un sistema esterni di autenticare e interagire con AEM as a Cloud Service tramite HTTP. Le credenziali del servizio non vengono inviate ad AEM per l’autenticazione, ma l’applicazione esterna le utilizza per generare un JWT, che viene scambiato con le API di Adobe IMS _per_ un token di accesso, che può quindi essere utilizzato per autenticare le richieste HTTP in AEM as a Cloud Service.

+ [Come utilizzare le credenziali del servizio](./service-credentials.md)

## Risorse aggiuntive

+ [Scaricare l’applicazione di esempio](./assets/aem-guides_token-authentication-external-application.zip)
+ Altri esempi di codice per la creazione e lo scambio di JWT
   + [Esempi di codici Node.js, Java, Python, C#.NET e PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)
   + [Esempio di codice basato su JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
