---
title: Connetti AEM con la proprietà tag utilizzando IMS
description: Scopri come connettersi AEM con Tag Property utilizzando la configurazione IMS in AEM. Questa configurazione autentica AEM con l’API Launch e consente AEM comunicare tramite le API Launch per accedere alle proprietà dei tag.
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 2%

---

# Connetti AEM con la proprietà tag utilizzando IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>Il processo di ridenominazione di Adobe Experience Platform Launch come set di tecnologie di raccolta dati è in corso di implementazione nell’interfaccia utente, nel contenuto e nella documentazione del prodotto AEM, pertanto il termine Launch è ancora in uso qui.

Scopri come connettersi AEM con Tag Property utilizzando la configurazione IMS (Identity Management System) in AEM. Questa configurazione autentica AEM con l’API Launch e consente AEM comunicare tramite le API Launch per accedere alle proprietà dei tag.

## Creare o riutilizzare la configurazione IMS

La configurazione IMS tramite il progetto Adobe Developer Console è necessaria per integrare AEM con la nuova proprietà tag creata. Questa configurazione consente AEM comunicare con l’applicazione Tag utilizzando le API di Launch e IMS gestisce l’aspetto relativo alla sicurezza di questa integrazione.

Ogni volta che viene eseguito il provisioning di un ambiente AEM come Cloud Service, vengono create automaticamente alcune configurazioni IMS come Asset compute, Adobe Analytics e Adobe Launch. Creazione automatica **Adobe Launch** È possibile utilizzare la configurazione IMS o creare una nuova configurazione IMS se utilizzi AEM ambiente 6.X.

Revisione creata automaticamente **Adobe Launch** Configurazione IMS tramite i passaggi seguenti.

1. In AEM apri il menu **Strumenti**

1. Nella sezione Sicurezza , seleziona Configurazioni Adobe IMS.

1. Seleziona la **Adobe Launch** scheda e fai clic su **Proprietà**, esamina i dettagli da **Certificato** e **Account** schede. Quindi fai clic su **Annulla** per tornare senza modificare i dettagli creati automaticamente.

1. Seleziona la **Adobe Launch** scheda e questa volta fai clic su **Verifica stato**, dovresti visualizzare le **Completato** come sotto.

   ![Configurazione di Adobe Launch Healthy IMS](assets/adobe-launch-healthy-ims-config.png)


## Passaggi successivi

[Creare una configurazione di Cloud Service Launch in AEM](create-aem-launch-cloud-service.md)
