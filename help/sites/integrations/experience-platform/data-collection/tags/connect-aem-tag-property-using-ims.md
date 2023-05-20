---
title: Connettere l’AEM con la proprietà Tag utilizzando IMS
description: Scopri come collegare l’AEM con Tag Property utilizzando la configurazione IMS nell’AEM. Questa configurazione autentica l’AEM con l’API Launch e consente all’AEM di comunicare tramite le API Launch per accedere alle proprietà Tag.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 2%

---

# Connettere l’AEM con la proprietà Tag utilizzando IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>Il processo di ridenominazione di Adobe Experience Platform Launch come set di tecnologie di raccolta dati è in fase di implementazione nell’interfaccia utente, nel contenuto e nella documentazione del prodotto AEM, pertanto il termine Launch viene ancora utilizzato qui.

Scopri come collegare l’AEM con la proprietà Tag utilizzando la configurazione IMS (Identity Management System) nell’AEM. Questa configurazione autentica l’AEM con l’API Launch e consente all’AEM di comunicare tramite le API Launch per accedere alle proprietà Tag.

## Creare o riutilizzare la configurazione IMS

Per integrare l’AEM con la nuova proprietà Tag è necessaria la configurazione IMS utilizzando il progetto della console Adobe Developer. Questa configurazione consente all’AEM di comunicare con l’applicazione Tags utilizzando le API Launch e IMS gestisce l’aspetto della sicurezza di questa integrazione.

Ogni volta che viene eseguito il provisioning di un ambiente AEM come Cloud Service, vengono create automaticamente alcune configurazioni IMS come Asset compute, Adobe Analytics e Adobe Launch. Creazione automatica **Adobe lancio** È possibile utilizzare la configurazione IMS oppure è necessario creare una nuova configurazione IMS se si utilizza l’ambiente AEM 6.X.

Revisione creata automaticamente **Adobe lancio** Configurazione IMS tramite i passaggi seguenti.

1. In AEM apri il menu **Strumenti**

1. Nella sezione Sicurezza, seleziona Configurazioni Adobe IMS.

1. Seleziona la **Adobe lancio** e fai clic su **Proprietà**, controlla i dettagli da **Certificato** e **Account** schede. Quindi fai clic su **Annulla** per tornare senza modificare i dettagli creati automaticamente.

1. Seleziona la **Adobe lancio** e questa volta fai clic **Verifica stato**, dovresti visualizzare **Completato** messaggio simile al seguente.

   ![Adobe di configurazione IMS integra per Launch](assets/adobe-launch-healthy-ims-config.png)


## Passaggi successivi

[Creare una configurazione del Cloud Service Launch nell’AEM](create-aem-launch-cloud-service.md)
