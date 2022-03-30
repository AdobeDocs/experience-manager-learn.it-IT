---
title: Configurare account e servizi per l'estensibilità di Asset compute
description: Lo sviluppo di processi di lavoro Asset compute richiede l’accesso ad account e servizi, tra cui AEM as a Cloud Service, Generatore di app e archiviazione cloud forniti da Microsoft o Amazon.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 1%

---

# Configurare account e servizi

Questa esercitazione richiede il provisioning e l’accesso ai seguenti servizi tramite l’Adobe ID dello studente.

Tutti i servizi Adobe devono essere accessibili tramite la stessa organizzazione Adobe utilizzando il tuo Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + Il provisioning può richiedere da 2 a 10 giorni
+ archiviazione cloud
   + [Archiviazione BLOB di Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Assicurati di avere accesso a tutti i servizi di cui sopra, prima di continuare questa esercitazione.
> 
> Rivedi le sezioni seguenti su come impostare e fornire i servizi richiesti.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

È necessario accedere a un ambiente AEM as a Cloud Service per configurare i profili di elaborazione AEM Assets per richiamare il processo di lavoro di Asset compute personalizzato.

È possibile utilizzare un programma sandbox o un ambiente di sviluppo non sandbox.

Tieni presente che un SDK AEM locale non è sufficiente per completare questa esercitazione, in quanto l’SDK AEM locale non può comunicare con i microservizi di Asset compute, ma è necessario un vero ambiente as a Cloud Service AEM.

## App Builder{#app-builder}

La [App Builder](https://developer.adobe.com/app-builder/) framework viene utilizzato per creare e distribuire azioni personalizzate su Adobe I/O Runtime, la piattaforma senza server Adobe. AEM progetti di Asset compute sono progetti App Builder appositamente costruiti che si integrano con AEM Assets tramite Profili elaborazione e consentono di accedere ed elaborare i file binari delle risorse.

Per accedere ad App Builder, iscriviti all’anteprima.

1. [Accedi alla versione di prova di App Builder](https://developer.adobe.com/app-builder/trial/).
1. Attendi circa 2-10 giorni finché non ricevi una notifica via e-mail prima di continuare l’esercitazione.
   + Se non sei sicuro di aver effettuato il provisioning, continua con i passaggi successivi e se non sei in grado di creare un __App Builder__ progetto in [Console per sviluppatori di Adobe](https://developer.adobe.com/console/) non è ancora stato effettuato il provisioning.

## archiviazione cloud

L’archiviazione cloud è necessaria per lo sviluppo locale dei progetti di Asset compute.

Quando i processi di lavoro di Asset compute vengono distribuiti in Adobe I/O Runtime per un uso diretto da AEM as a Cloud Service, questo spazio di archiviazione cloud non è strettamente necessario in quanto AEM fornisce l’archiviazione cloud da cui la risorsa viene letta e trasformata in scrittura.

### Archiviazione BLOB di Microsoft Azure{#azure-blob-storage}

Se non disponi già dell’accesso all’archiviazione BLOB di Microsoft Azure, registrati per un [account gratuito di 12 mesi](https://azure.microsoft.com/en-us/free/).

Questa esercitazione utilizzerà l’archiviazione BLOB di Azure, tuttavia [Amazon S3](#amazon-s3) possono essere utilizzate anche solo varianti minori dell’esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Click-through del provisioning dell’archiviazione BLOB di Azure (nessun audio)_

1. Accedi al tuo [Account Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Passa a __Account di archiviazione__ Sezione servizi di Azure
1. Tocca __+ Aggiungi__ per creare un nuovo account di archiviazione BLOB
1. Crea un nuovo __Gruppo di risorse__ se necessario, ad esempio: `aem-as-a-cloud-service`
1. Fornisci un __Nome account di archiviazione__ ad esempio: `aemguideswkndassetcomput`
   + La __Nome account di archiviazione__ verrà utilizzato per [configurazione dell’archiviazione cloud](../develop/environment-variables.md) per lo strumento di sviluppo Asset compute locale
   + La __Tasti di accesso__ associati all&#39;account di archiviazione sono necessari anche quando [configurazione dell’archiviazione cloud](../develop/environment-variables.md).
1. Lascia tutto il resto come predefinito e tocca il __Rivedi e crea__ pulsante
   + Facoltativamente, seleziona la __posizione__ vicino a te.
1. Rivedi la richiesta di provisioning per la correttezza e tocca __Crea__ pulsante se saturo

### Amazon S3{#amazon-s3}

Utilizzo [Archiviazione BLOB di Microsoft Azure](#azure-blob-storage) si consiglia tuttavia di completare questa esercitazione, [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) può essere utilizzato anche.

Se utilizzi l’archiviazione Amazon S3, specifica le credenziali di archiviazione cloud Amazon S3 quando [configurazione delle variabili di ambiente del progetto](../develop/environment-variables.md#amazon-s3).

Se hai bisogno di eseguire il provisioning dell’archiviazione cloud per questa esercitazione, ti consigliamo di utilizzare [Archiviazione BLOB di Azure](#azure-blob-storage).
