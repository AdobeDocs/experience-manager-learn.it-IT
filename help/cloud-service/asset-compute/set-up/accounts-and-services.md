---
title: Impostare account e servizi per l'estensibilità di Asset compute
description: Lo sviluppo di lavoratori Asset compute richiede l’accesso ad account e servizi tra cui AEM as a Cloud Service, App Builder e l’archiviazione cloud fornita da Microsoft o Amazon.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 253
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 1%

---

# Configurare account e servizi

Questo tutorial richiede che i seguenti servizi siano disponibili e accessibili tramite l’Adobe ID dell’Allievo.

Tutti i servizi Adobe devono essere accessibili tramite la stessa organizzazione Adobe, utilizzando il tuo Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + Il provisioning può richiedere da 2 a 10 giorni
+ Archiviazione cloud
   + [Archiviazione BLOB di Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Assicurati di avere accesso a tutti i servizi sopra menzionati, prima di continuare con questa esercitazione.
> 
> Leggi le sezioni seguenti su come impostare ed eseguire il provisioning dei servizi richiesti.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Per configurare i profili di elaborazione AEM Assets in modo da richiamare il processo di lavoro Asset compute personalizzato, è necessario accedere a un ambiente AEM as a Cloud Service.

È consigliabile poter utilizzare un programma sandbox o un ambiente di sviluppo non sandbox.

Tieni presente che un SDK per AEM locale non è sufficiente per completare questa esercitazione, poiché l’SDK per AEM locale non è in grado di comunicare con i microservizi Asset compute, è necessario un ambiente AEM as a Cloud Service.

## App Builder{#app-builder}

Il [App Builder](https://developer.adobe.com/app-builder/) Il framework viene utilizzato per creare e distribuire azioni personalizzate in Adobe I/O Runtime, la piattaforma senza server di Adobe. I progetti di Asset compute dell’AEM sono progetti App Builder appositamente creati che si integrano con AEM Assets tramite i Profili di elaborazione e forniscono la possibilità di accedere ed elaborare i dati binari delle risorse.

Per accedere ad App Builder, iscriviti per l’anteprima.

1. [Registrati alla versione di prova di App Builder](https://developer.adobe.com/app-builder/trial/).
1. Attendi circa 2-10 giorni prima di continuare l’esercitazione e ricevi una notifica via e-mail che informa del provisioning.
   + Se non sei sicuro di aver effettuato il provisioning, continua con i passaggi successivi e se non riesci a creare una __App Builder__ progetto in [Console Adobe Developer](https://developer.adobe.com/console/) non è ancora stato eseguito il provisioning.

## Archiviazione cloud

L’archiviazione cloud è necessaria per lo sviluppo locale di progetti Asset compute.

Quando i processi di lavoro Asset compute vengono distribuiti in Adobe I/O Runtime per l’uso diretto da AEM as a Cloud Service, questa archiviazione cloud non è strettamente necessaria in quanto AEM fornisce l’archiviazione cloud da cui la risorsa viene letta e in cui viene scritto il rendering.

### Archiviazione BLOB di Microsoft Azure{#azure-blob-storage}

Se non disponi già dell’accesso all’archiviazione BLOB di Microsoft Azure, registrati per [account gratuito per 12 mesi](https://azure.microsoft.com/en-us/free/).

Tuttavia, questa esercitazione utilizzerà Azure Blob Storage [Amazon S3](#amazon-s3) può essere utilizzato anche solo come variante minore dell’esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Click-through del provisioning di Azure Blob Storage (nessun audio)_

1. Accedi al tuo [Account di Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Accedi a __Account di archiviazione__ Sezione Servizi di Azure
1. Tocca __+ Aggiungi__ per creare un nuovo account di archiviazione BLOB
1. Crea un nuovo __Gruppo di risorse__ secondo necessità, ad esempio: `aem-as-a-cloud-service`
1. Fornisci un __Nome account di archiviazione__, ad esempio: `aemguideswkndassetcomput`
   + Il __Nome account di archiviazione__  utilizzato per [configurazione dell’archiviazione cloud](../develop/environment-variables.md) nello strumento di sviluppo Asset compute locale
   + Il __Chiavi di accesso__ associati all&#39;account di archiviazione sono necessari anche quando [configurazione dell’archiviazione cloud](../develop/environment-variables.md).
1. Lascia tutto il resto come predefinito, quindi tocca il __Revisione e creazione__ pulsante
   + Facoltativamente, seleziona la __posizione__ vicino a te.
1. Verifica la correttezza della richiesta di provisioning e tocca __Crea__ pulsante se soddisfatto

### Amazon S3{#amazon-s3}

Utilizzo di [Archiviazione BLOB di Microsoft Azure](#azure-blob-storage) Tuttavia, si consiglia di completare questa esercitazione [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) può essere utilizzato anche.

Se utilizzi l’archiviazione Amazon S3, specifica le credenziali dell’archiviazione cloud Amazon S3 quando [configurazione delle variabili di ambiente del progetto](../develop/environment-variables.md#amazon-s3).

Per eseguire il provisioning dell’archiviazione cloud appositamente per questa esercitazione, consigliamo di utilizzare [Archiviazione BLOB di Azure](#azure-blob-storage).
