---
title: Configurare account e servizi per l'estensibilità di elaborazione delle risorse
description: Lo sviluppo di Asset Compute Workers richiede l'accesso ad account e servizi, tra cui AEM come Cloud Service,  Adobe Project Firefly e l'archiviazione cloud forniti da Microsoft o  Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# Configurare account e servizi

Questa esercitazione richiede il provisioning e l&#39;accesso ai seguenti servizi tramite l&#39;Adobe ID  dello studente.

Tutti i servizi  Adobe devono essere accessibili tramite la stessa organizzazione  Adobe, utilizzando l&#39;Adobe ID .

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [progetto Adobe FireFly](#adobe-project-firefly)
   + Il provisioning può richiedere tra 2 e 10 giorni
+ Archiviazione cloud
   + [Archiviazione BLOB di Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Prima di continuare con questa esercitazione, assicurati di poter accedere a tutti i servizi indicati sopra.
> 
> Consultate le sezioni seguenti su come impostare e fornire i servizi richiesti.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

È necessario accedere a un AEM come ambiente di Cloud Service per configurare  profili di elaborazione AEM Assets per richiamare l’applicazione personalizzata Asset Compute.

Idealmente è disponibile per l&#39;uso un programma sandbox o un ambiente di sviluppo non sandbox.

Un SDK AEM locale non è sufficiente per completare questa esercitazione, in quanto l’SDK AEM locale non può comunicare con Asset Compute microservices, ma è necessario un AEM vero come ambiente Cloud Service.

##  progetto Adobe{#adobe-project-firefly}

Il framework [Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) viene utilizzato per creare e distribuire applicazioni personalizzate ad Adobe I/O Runtime,  Adobe piattaforma senza server. AEM le applicazioni Asset Compute sono applicazioni Firefly appositamente costruite che si integrano con  AEM Assets tramite Profili di elaborazione e forniscono la possibilità di accedere ed elaborare i file binari di risorse.

Per accedere a Project Firefly, registratevi per l&#39;anteprima.

1. [Iscriviti a Project Firefly Preview](https://adobeio.typeform.com/to/obqgRm).
1. Attendete circa 2-10 giorni prima di ricevere via e-mail le notifiche per le quali avete effettuato il provisioning prima di continuare l&#39;esercitazione.
   + Se non sei sicuro di aver effettuato il provisioning, continua con i passaggi successivi e se non sei in grado di creare un progetto __Project Firefly__ in [Adobe Developer Console](https://console.adobe.io) non hai ancora effettuato il provisioning.

## Archiviazione cloud

L&#39;archiviazione cloud è necessaria per lo sviluppo locale delle applicazioni Asset Compute.

Quando le applicazioni Asset Compute vengono distribuite sull’Adobe I/O Runtime per l’uso diretto da AEM come Cloud Service, l’archiviazione cloud non è strettamente necessaria in quanto AEM fornisce l’archiviazione cloud da cui la risorsa viene letta e trasformata.

### Archiviazione BLOB di Microsoft Azure{#azure-blob-storage}

Se non si dispone già dell&#39;accesso a Microsoft Azure Blob Storage, registrarsi per un account [](https://azure.microsoft.com/en-us/free/)gratuito di 12 mesi.

Questa esercitazione utilizzerà Azure Blob Storage, tuttavia [Amazon S3](#amazon-s3) può essere utilizzato e solo lievi varianti dell&#39;esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)
_Click-through del provisioning di Azure Blob Storage (nessun audio)_


1. Accedi al tuo account [](https://azure.microsoft.com/en-us/account/)Microsoft Azure.
1. Passare alla sezione Servizi di Azure __Account__ di archiviazione
1. Tocca __+ Aggiungi__ per creare un nuovo account Blob Storage
1. Crea un nuovo gruppo __di__ risorse in base alle esigenze, ad esempio: `aem-as-a-cloud-service`
1. Specificare un nome __account__ di archiviazione, ad esempio: `aemguideswkndassetcomput`
   + Il nome __dell&#39;account__ di archiviazione verrà utilizzato per [configurare l&#39;archiviazione](../develop/environment-variables.md) cloud per lo strumento di sviluppo di calcolo risorse locale
   + Le chiavi __di__ accesso associate all&#39;account di archiviazione sono necessarie anche per la [configurazione dell&#39;archiviazione](../develop/environment-variables.md)cloud.
1. Lasciate tutto il resto come predefinito, quindi toccate il pulsante __Rivedi + Crea__ .
   + Se necessario, selezionate la __posizione__ che vi sta vicino.
1. Esaminate la richiesta di provisioning per verificarne la correttezza e toccate il pulsante __Crea__ , se soddisfatto

###  Amazon S3{#amazon-s3}

Per completare l&#39;esercitazione si consiglia di utilizzare [Microsoft Azure Blob Storage](#azure-blob-storage) , ma [è possibile utilizzare anche Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) .

Se utilizzate  storage Amazon S3, specificate le credenziali  di archiviazione cloud Amazon S3 al momento della [configurazione delle variabili](../develop/environment-variables.md#amazon-s3)di ambiente del progetto.

Se è necessario eseguire il provisioning dell&#39;archiviazione cloud per questa esercitazione, si consiglia di utilizzare [Azure Blob Storage](#azure-blob-storage).
