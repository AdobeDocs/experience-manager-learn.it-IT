---
title: Configurare account e servizi per l’estensibilità di Asset Compute
description: Lo sviluppo dei processi di lavoro Asset Compute richiede l’accesso a account e servizi, tra cui AEM as a Cloud Service, Adobe Project Firefly e l’archiviazione cloud fornita da Microsoft o Amazon.
feature: Microservizi Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrazioni, Sviluppo
role: Developer (Sviluppatore)
level: Intermedio, esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# Configurare account e servizi

Questa esercitazione richiede il provisioning e l’accesso ai seguenti servizi tramite l’Adobe ID dello studente.

Tutti i servizi Adobe devono essere accessibili tramite la stessa organizzazione Adobe utilizzando il tuo Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
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

È necessario accedere a un ambiente AEM as a Cloud Service per configurare i profili di elaborazione di AEM Assets per richiamare il processo di lavoro Asset Compute personalizzato.

È possibile utilizzare un programma sandbox o un ambiente di sviluppo non sandbox.

Tieni presente che un SDK AEM locale non è sufficiente per completare questa esercitazione, poiché l’SDK AEM locale non può comunicare con i microservizi Asset Compute, ma è necessario un vero ambiente AEM as a Cloud Service.

## Adobe Project Firefly{#adobe-project-firefly}

Il framework [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) viene utilizzato per creare e distribuire azioni personalizzate per Adobe I/O Runtime, la piattaforma senza server di Adobe. I progetti AEM Asset Compute sono progetti Firefly appositamente costruiti che si integrano con AEM Assets tramite Profili elaborazione e consentono di accedere ed elaborare i binari delle risorse.

Per accedere a Project Firefly, iscriviti all&#39;anteprima.

1. [Iscriviti a Project Firefly preview](https://adobeio.typeform.com/to/obqgRm).
1. Attendi circa 2-10 giorni finché non ricevi una notifica via e-mail prima di continuare l’esercitazione.
   + Se non sei sicuro di aver effettuato il provisioning, continua con i passaggi successivi e se non riesci a creare un progetto __Project Firefly__ in [Adobe Developer Console](https://console.adobe.io) non hai ancora effettuato il provisioning.

## archiviazione cloud

L’archiviazione cloud è necessaria per lo sviluppo locale dei progetti Asset Compute.

Quando i processi di lavoro di Asset Compute vengono implementati in Adobe I/O Runtime per l’utilizzo diretto da parte di AEM as a Cloud Service, tale archiviazione cloud non è strettamente necessaria in quanto AEM fornisce l’archiviazione cloud da cui viene letta e scritta la risorsa.

### Archiviazione BLOB di Microsoft Azure{#azure-blob-storage}

Se non disponi già dell&#39;accesso all&#39;archiviazione BLOB di Microsoft Azure, registrati per un account [gratuito di 12 mesi](https://azure.microsoft.com/en-us/free/).

Questa esercitazione utilizzerà l’archiviazione BLOB di Azure, tuttavia è possibile utilizzare [Amazon S3](#amazon-s3) e solo una variazione minore dell’esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Click-through del provisioning dell’archiviazione BLOB di Azure (nessun audio)_


1. Accedi al tuo [account Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Passa alla sezione __Account di archiviazione__ Servizi di Azure
1. Tocca __+ Aggiungi__ per creare un nuovo account Blob Storage
1. Crea un nuovo __gruppo di risorse__ in base alle esigenze, ad esempio: `aem-as-a-cloud-service`
1. Fornire un __nome account di archiviazione__, ad esempio: `aemguideswkndassetcomput`
   + Il __nome dell’account di archiviazione__ verrà utilizzato per [configurare l’archiviazione cloud](../develop/environment-variables.md) per lo strumento di sviluppo di Asset Compute locale
   + Le __chiavi di accesso__ associate all&#39;account di archiviazione sono necessarie anche durante la [configurazione dell&#39;archiviazione cloud](../develop/environment-variables.md).
1. Lascia tutto il resto come predefinito e tocca il pulsante __Rivedi + crea__
   + Facoltativamente, seleziona la __posizione__ vicina a te.
1. Rivedi la richiesta di provisioning per la correttezza e tocca il pulsante __Crea__ se soddisfatto

### Amazon S3{#amazon-s3}

Per completare questa esercitazione, si consiglia di utilizzare [Archiviazione BLOB di Microsoft Azure](#azure-blob-storage), ma è anche possibile utilizzare [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card).

Se utilizzi l’archiviazione Amazon S3, specifica le credenziali di archiviazione cloud Amazon S3 durante la [configurazione delle variabili di ambiente del progetto](../develop/environment-variables.md#amazon-s3).

Se hai bisogno di eseguire il provisioning dell&#39;archiviazione cloud per questa esercitazione, ti consigliamo di utilizzare [Azure Blob Storage](#azure-blob-storage).
