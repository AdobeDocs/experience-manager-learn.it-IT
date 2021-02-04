---
title: Autenticazione per AEM come Cloud Service da un'applicazione esterna
description: Scopri in che modo un'applicazione esterna può autenticare e interagire in modo programmatico con AEM come Cloud Service via HTTP utilizzando i Token di accesso allo sviluppo locale e le credenziali del servizio.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: 733382dc0e0ca14d4bd6e49174ba33f8d7fc517d
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---


# Autenticazione basata su token da AEM come Cloud Service

Questa esercitazione spiega come un&#39;applicazione esterna può autenticarsi a livello di programmazione e interagire con per AEM come Cloud Service via HTTP utilizzando i token di accesso.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

## Prerequisiti

Prima di seguire questa esercitazione, accertatevi che siano presenti le seguenti risorse:

1. L&#39;accesso all&#39;am AEM come ambiente Cloud Service (preferibilmente un ambiente di sviluppo o un programma sandbox)
1. Iscrizione al AEM come servizi di authoring dell&#39;ambiente di Cloud Service AEM profilo di prodotto Amministratore
1. Iscrizione o accesso al proprio amministratore di organizzazione IMS del Adobe  (dovrà eseguire un&#39;inizializzazione una tantum delle [Credenziali del servizio](./service-credentials.md))
1. Ultimo [sito WKND](https://github.com/adobe/aem-guides-wknd) implementato nell&#39;ambiente di Cloud Service

## Panoramica dell’applicazione esterna

Questa esercitazione utilizza una [semplice applicazione Node.js](./assets/aem-guides_token-authentication-external-application.zip) eseguita dalla riga di comando per aggiornare i metadati delle risorse in AEM come Cloud Service utilizzando [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

Il flusso di esecuzione dell&#39;applicazione Node.js è il seguente:

![Applicazione esterna](./assets/overview/external-application.png)

1. L&#39;applicazione Node.js viene chiamata dalla riga di comando
1. I parametri della riga di comando definiscono:
   + Il AEM come host del servizio Autore di Cloud Service a cui connettersi (`aem`)
   + La cartella di risorse AEM le cui risorse verranno aggiornate (`folder`)
   + Proprietà e valore dei metadati da aggiornare (`propertyName` e `propertyValue`)
   + Percorso locale del file che fornisce le credenziali necessarie per accedere AEM come Cloud Service (`file`)
1. Il token di accesso utilizzato per l&#39;autenticazione a AEM è derivato dal file JSON fornito tramite il parametro della riga di comando `file`

   a. Se le credenziali di servizio utilizzate per lo sviluppo non locale vengono fornite nel file JSON (`file`), il token di accesso viene recuperato  API IMS Adobe
1. L&#39;applicazione utilizza il token di accesso per accedere AEM ed elencare tutte le risorse nella cartella specificata nel parametro della riga di comando `folder`
1. Per ogni risorsa nella cartella, l&#39;applicazione aggiorna i metadati in base al nome e al valore della proprietà specificati nei parametri della riga di comando `propertyName` e `propertyValue`

Sebbene questa applicazione di esempio sia Node.js, queste interazioni possono essere sviluppate utilizzando linguaggi di programmazione diversi ed eseguite da altri sistemi esterni.

## Token di accesso allo sviluppo locale

I token di accesso allo sviluppo locale vengono generati per un AEM specifico come ambiente di Cloud Service e forniscono l&#39;accesso ai servizi Autore e Pubblica.  Questi token di accesso sono temporanei e devono essere utilizzati solo durante lo sviluppo di applicazioni o sistemi esterni che interagiscono con AEM via HTTP. Invece di dover ottenere e gestire le credenziali di assistenza, uno sviluppatore può generare rapidamente e facilmente un token di accesso temporaneo che consenta loro di sviluppare la propria integrazione.

+ [Come utilizzare il token di accesso allo sviluppo locale](./local-development-access-token.md)

## Credenziali del servizio

Le credenziali del servizio sono le credenziali unificate utilizzate in qualsiasi scenario non di sviluppo (ovviamente produzione) che facilitano l&#39;autenticazione e l&#39;interazione di un&#39;applicazione esterna o del sistema AEM come Cloud Service su HTTP. Le credenziali del servizio non vengono inviate a AEM per l&#39;autenticazione, ma l&#39;applicazione esterna le utilizza per generare un JWT, che viene scambiato con  API IMS del Adobe _per_ un token di accesso, che può essere utilizzato per autenticare le richieste HTTP da AEM come Cloud Service.

+ [Come utilizzare le credenziali del servizio](./service-credentials.md)

## Altro materiale di riferimento

+ [Scaricare l&#39;applicazione di esempio](./assets/aem-guides_token-authentication-external-application.zip)
+ Altri esempi di codice per la creazione e lo scambio di JWT
   + [Esempi di codice Node.js, Java, Python, C#.NET e PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Esempio di codice basato su JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
