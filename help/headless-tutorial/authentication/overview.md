---
title: Autenticazione in AEM as a Cloud Service da un’applicazione esterna
description: Scopri in che modo un’applicazione esterna può autenticare e interagire in modo programmatico con AEM as a Cloud Service tramite HTTP utilizzando i token di accesso allo sviluppo locale e le credenziali del servizio.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Senza testa, Integrazioni
role: Developer (Sviluppatore)
level: Intermedio, esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 0%

---


# Autenticazione basata su token per AEM as a Cloud Service

In questa esercitazione approfondisci come un’applicazione esterna può autenticare e interagire in modo programmatico con AEM as a Cloud Service tramite HTTP utilizzando i token di accesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Prerequisiti

Assicurati che quanto segue sia già presente prima di seguire questa esercitazione:

1. Accesso a un ambiente AEM as a Cloud Service (preferibilmente un ambiente di sviluppo o un programma sandbox)
1. Iscrizione al profilo di prodotto dell’ambiente Author dei servizi AEM Administrator dell’ambiente AEM as a Cloud Service
1. Iscrizione o accesso all’amministratore dell’organizzazione Adobe IMS (dovrà eseguire un’inizializzazione una tantum delle [Credenziali del servizio](./service-credentials.md))
1. Ultimo [sito WKND](https://github.com/adobe/aem-guides-wknd) implementato nell’ambiente Cloud Service

## Panoramica dell’applicazione esterna

Questa esercitazione utilizza una [semplice applicazione Node.js](./assets/aem-guides_token-authentication-external-application.zip) eseguita dalla riga di comando per aggiornare i metadati delle risorse su AEM as a Cloud Service utilizzando [API HTTP Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

Il flusso di esecuzione dell&#39;applicazione Node.js è il seguente:

![Applicazione esterna](./assets/overview/external-application.png)

1. L&#39;applicazione Node.js viene richiamata dalla riga di comando
1. I parametri della riga di comando definiscono:
   + L’host del servizio Author di AEM as a Cloud Service a cui connettersi (`aem`)
   + La cartella delle risorse AEM di cui verranno aggiornate le risorse (`folder`)
   + Proprietà e valore dei metadati da aggiornare (`propertyName` e `propertyValue`)
   + Percorso locale del file che fornisce le credenziali necessarie per accedere ad AEM as a Cloud Service (`file`)
1. Il token di accesso utilizzato per l’autenticazione in AEM deriva dal file JSON fornito tramite il parametro della riga di comando `file`

   a) Se le credenziali del servizio utilizzate per lo sviluppo non locale sono fornite nel file JSON (`file`), il token di accesso viene recuperato dalle API Adobe IMS
1. L’applicazione utilizza il token di accesso per accedere ad AEM ed elencare tutte le risorse nella cartella specificata nel parametro della riga di comando `folder`
1. Per ogni risorsa della cartella, l&#39;applicazione aggiorna i metadati in base al nome e al valore della proprietà specificati nei parametri della riga di comando `propertyName` e `propertyValue`

Anche se questa applicazione di esempio è Node.js, queste interazioni possono essere sviluppate utilizzando diversi linguaggi di programmazione ed eseguite da altri sistemi esterni.

## Token di accesso per lo sviluppo locale

I token di accesso allo sviluppo locale vengono generati per un ambiente AEM as a Cloud Service specifico e forniscono accesso ai servizi Author e Publish .  Questi token di accesso sono temporanei e devono essere utilizzati solo durante lo sviluppo di applicazioni o sistemi esterni che interagiscono con AEM tramite HTTP. Invece di uno sviluppatore che deve ottenere e gestire le credenziali di servizio del bonafide, possono generare rapidamente e facilmente un token di accesso temporaneo che consenta loro di sviluppare la loro integrazione.

+ [Come utilizzare il token di accesso allo sviluppo locale](./local-development-access-token.md)

## Credenziali del servizio

Le credenziali del servizio sono le credenziali per il collegamento utilizzate in qualsiasi scenario non di sviluppo, ovviamente in fase di produzione, che facilitano l’autenticazione e l’interazione di un’applicazione esterna o del sistema con AEM as a Cloud Service tramite HTTP. Le credenziali del servizio non vengono inviate ad AEM per l’autenticazione, ma l’applicazione esterna le utilizza per generare un JWT, che viene scambiato con le API di Adobe IMS _per_ un token di accesso, che può quindi essere utilizzato per autenticare le richieste HTTP ad AEM as a Cloud Service.

+ [Come utilizzare le credenziali del servizio](./service-credentials.md)

## Altro materiale di riferimento

+ [Scarica l’applicazione di esempio](./assets/aem-guides_token-authentication-external-application.zip)
+ Altri esempi di codice per la creazione e lo scambio di JWT
   + [Esempi di codice Node.js, Java, Python, C#.NET e PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Esempio di codice basato su JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
