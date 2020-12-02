---
title: Configurare account e servizi per l'estensibilità  Asset compute
description: Lo sviluppo  Asset compute richiede l'accesso a account e servizi, compresi AEM come Cloud Service,  progetto Firefly e l'archiviazione cloud forniti da Microsoft o  Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# Configurare account e servizi

Questa esercitazione richiede il provisioning e l&#39;accesso ai seguenti servizi tramite l&#39;Adobe ID  dello studente.

Tutti i servizi  Adobe devono essere accessibili tramite la stessa organizzazione  Adobe, utilizzando l&#39;Adobe ID .

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [ progetto Adobe FireFly](#adobe-project-firefly)
   + Il provisioning può richiedere tra 2 e 10 giorni
+ Archiviazione cloud
   + [Archiviazione BLOB di Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [ Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Prima di continuare con questa esercitazione, assicurati di poter accedere a tutti i servizi indicati sopra.
> 
> Consultate le sezioni seguenti su come impostare e fornire i servizi richiesti.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

È necessario accedere a un AEM come ambiente di Cloud Service per configurare  profili di elaborazione AEM Assets in modo da richiamare il lavoratore  Asset compute personalizzato.

Idealmente è disponibile per l&#39;uso un programma sandbox o un ambiente di sviluppo non sandbox.

Un SDK AEM locale non è sufficiente per completare questa esercitazione, in quanto l&#39;SDK AEM locale non può comunicare con  microservizi Asset compute, ma è necessario un AEM vero come ambiente Cloud Service.

##  progetto Adobe Firefly{#adobe-project-firefly}

Il framework [ Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) Adobe viene utilizzato per creare e distribuire azioni personalizzate su Adobe I/O Runtime,  Adobe piattaforma server. AEM progetti di Asset compute sono progetti Firefly appositamente costruiti che si integrano con  AEM Assets tramite Profili di elaborazione e forniscono la possibilità di accedere ed elaborare file binari di risorse.

Per accedere a Project Firefly, registratevi per l&#39;anteprima.

1. [Iscriviti a Project Firefly Preview](https://adobeio.typeform.com/to/obqgRm).
1. Attendete circa 2-10 giorni prima di ricevere via e-mail le notifiche per le quali avete effettuato il provisioning prima di continuare l&#39;esercitazione.
   + Se non sei sicuro di aver effettuato il provisioning, continua con i passaggi successivi e se non sei in grado di creare un progetto __Project Firefly__ in [ Adobe Developer Console](https://console.adobe.io) non hai ancora effettuato il provisioning.

## Archiviazione cloud

L&#39;archiviazione cloud è necessaria per lo sviluppo locale di progetti  Asset compute.

Quando  Asset compute vengono distribuiti all’Adobe I/O Runtime per l’uso diretto da AEM come Cloud Service, l’archiviazione cloud non è strettamente necessaria in quanto AEM fornisce l’archiviazione cloud da cui la risorsa viene letta e trasformata.

### Archiviazione BLOB di Microsoft Azure{#azure-blob-storage}

Se non si dispone già dell&#39;accesso a Microsoft Azure Blob Storage, effettuare la registrazione per un account gratuito di 12 mesi [.](https://azure.microsoft.com/en-us/free/)

Questa esercitazione utilizzerà Azure Blob Storage, tuttavia è possibile utilizzare [ Amazon S3](#amazon-s3) e solo lievi varianti dell&#39;esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Click-through del provisioning di Azure Blob Storage (nessun audio)_


1. Accedi al tuo [account Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Passare alla sezione __Account di archiviazione__ Servizi di Azure
1. Toccate __+ Aggiungi__ per creare un nuovo account Blob Storage
1. Crea un nuovo __gruppo di risorse__ in base alle esigenze, ad esempio: `aem-as-a-cloud-service`
1. Specificare un __nome account di archiviazione__, ad esempio: `aemguideswkndassetcomput`
   + Il __nome dell&#39;account di storage__ verrà utilizzato per [configurare l&#39;archiviazione cloud](../develop/environment-variables.md) per lo strumento di sviluppo del Asset compute  locale
   + Le __chiavi di accesso__ associate all&#39;account di archiviazione sono inoltre necessarie per [configurare l&#39;archiviazione cloud](../develop/environment-variables.md).
1. Lasciate tutto il resto come predefinito, quindi toccate il pulsante __Rivedi + crea__
   + Facoltativamente, selezionare la __posizione__ accanto all&#39;utente.
1. Rivedete la richiesta di provisioning per la correttezza, quindi toccate il pulsante __Crea__ se soddisfatto

###  Amazon S3{#amazon-s3}

Per completare questa esercitazione si consiglia di utilizzare [Microsoft Azure Blob Storage](#azure-blob-storage), tuttavia è possibile utilizzare anche [ Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card).

Se utilizzate  storage Amazon S3, specificate le credenziali  di archiviazione cloud Amazon S3 durante la [configurazione delle variabili di ambiente del progetto](../develop/environment-variables.md#amazon-s3).

Se è necessario eseguire il provisioning dell&#39;archiviazione cloud per questa esercitazione, si consiglia di utilizzare [Azure Blob Storage](#azure-blob-storage).
