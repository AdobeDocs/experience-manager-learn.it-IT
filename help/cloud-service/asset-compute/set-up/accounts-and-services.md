---
title: Impostare account e servizi per l'estensibilità di Asset compute
description: Lo sviluppo di processi di lavoro Asset Compute richiede l’accesso ad account e servizi tra cui AEM as a Cloud Service, App Builder e l’archiviazione cloud fornita da Microsoft o Amazon.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 2%

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

È necessario accedere a un ambiente AEM as a Cloud Service per configurare i profili di elaborazione di AEM Assets in modo da richiamare il processo di lavoro Asset Compute personalizzato.

È consigliabile poter utilizzare un programma sandbox o un ambiente di sviluppo non sandbox.

Tieni presente che un SDK per AEM locale non è sufficiente per completare questa esercitazione, poiché l’SDK per AEM locale non è in grado di comunicare con i microservizi Asset Compute, è necessario un vero ambiente AEM as a Cloud Service.

## App Builder{#app-builder}

Il framework [App Builder](https://developer.adobe.com/app-builder/) viene utilizzato per la creazione e la distribuzione di azioni personalizzate in Adobe I/O Runtime, la piattaforma senza server di Adobe. I progetti di Asset compute AEM sono progetti App Builder appositamente creati che si integrano con AEM Assets tramite i Profili di elaborazione e forniscono la possibilità di accedere ed elaborare i dati binari delle risorse.

Per accedere ad App Builder, iscriviti per l’anteprima.

1. [Iscriviti alla versione di prova di App Builder](https://developer.adobe.com/app-builder/trial/).
1. Attendi circa 2-10 giorni prima di continuare l’esercitazione e ricevi una notifica via e-mail che informa del provisioning.
   + Se non sei sicuro di aver eseguito il provisioning, continua con i passaggi successivi e se non riesci a creare un progetto __App Builder__ in [Adobe Developer Console](https://developer.adobe.com/console/) non hai ancora eseguito il provisioning.

## Archiviazione cloud

L’archiviazione cloud è necessaria per lo sviluppo locale di progetti Asset Compute.

Quando i processi di lavoro Asset Compute vengono distribuiti in Adobe I/O Runtime per l’utilizzo diretto da parte di AEM as a Cloud Service, questa archiviazione cloud non è strettamente necessaria in quanto AEM fornisce l’archiviazione cloud da cui la risorsa viene letta e in cui viene scritto il rendering.

### Archiviazione BLOB di Microsoft Azure{#azure-blob-storage}

Se non disponi già dell&#39;accesso all&#39;archiviazione BLOB di Microsoft Azure, registrati per un account di [12 mesi gratuito](https://azure.microsoft.com/en-us/free/).

Questo tutorial utilizzerà Azure Blob Storage, tuttavia [Amazon S3](#amazon-s3) può essere utilizzato come solo variante secondaria dell&#39;esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Click-through del provisioning di Azure Blob Storage (nessun audio)_

1. Accedi al tuo [account Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Passare alla sezione __Account di archiviazione__ Servizi di Azure
1. Tocca __+ Aggiungi__ per creare un nuovo account di archiviazione BLOB
1. Crea un nuovo __gruppo di risorse__ in base alle esigenze, ad esempio: `aem-as-a-cloud-service`
1. Fornisci un __nome account di archiviazione__, ad esempio: `aemguideswkndassetcomput`
   + Il __nome account di archiviazione__ utilizzato per [la configurazione dell&#39;archiviazione cloud](../develop/environment-variables.md) nello strumento di sviluppo Asset compute locale
   + Le __chiavi di accesso__ associate all&#39;account di archiviazione sono necessarie anche durante la [configurazione dell&#39;archiviazione cloud](../develop/environment-variables.md).
1. Lascia tutto il resto come predefinito e tocca il pulsante __Rivedi + crea__
   + Se necessario, seleziona la __posizione__ vicina.
1. Verifica la correttezza della richiesta di provisioning e tocca il pulsante __Crea__ se soddisfatto

### Amazon S3{#amazon-s3}

Per completare questa esercitazione si consiglia di utilizzare [Archiviazione BLOB di Microsoft Azure](#azure-blob-storage), tuttavia è possibile utilizzare anche [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card).

Se utilizzi l&#39;archiviazione Amazon S3, specifica le credenziali dell&#39;archiviazione cloud Amazon S3 durante [la configurazione delle variabili di ambiente del progetto](../develop/environment-variables.md#amazon-s3).

Se devi eseguire il provisioning dell&#39;archiviazione cloud appositamente per questa esercitazione, ti consigliamo di utilizzare [Archiviazione BLOB di Azure](#azure-blob-storage).
