---
title: Creare un account Cloud Service di Adobe Target in AEM
description: Integra Adobe Experience Manager as a Cloud Service con Adobe Target utilizzando Cloud Service e l’autenticazione IMS di Adobe.
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 8ebaba01d4b470a1ca7c1630f55b756796f3640d
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 8%

---

# Creare un account Cloud Service di Adobe Target {#adobe-target-cloud-service}

Il video seguente illustra come collegare AEM as a Cloud Service ad Adobe Target.

Questa integrazione consente al servizio di authoring di AEM di comunicare direttamente con Adobe Target e di inviare frammenti di esperienza da AEM a Target come offerte.  Questa integrazione *non* aggiunge Adobe Target JavaScript (AT.js) alle pagine Web di AEM Sites, per tale integrazione [AEM e tag utilizzando l&#39;estensione Target](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md).

>[!WARNING]
>
>Il video mostra un metodo di autenticazione JWT obsoleto per collegare AEM ad Adobe Target. Tuttavia, il metodo consigliato è quello di utilizzare il metodo di autenticazione server-to-server OAuth. Per ulteriori informazioni, consulta [Migrazione delle credenziali JWT-OAuth per AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration). Stiamo lavorando all’aggiornamento del video per riflettere questo cambiamento.


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Esiste un problema noto con la configurazione di Adobe Target Cloud Services mostrato nel video. Fino a quando questo problema non viene risolto, segui gli stessi passaggi nel video ma utilizza la [configurazione legacy di Adobe Target Cloud Services](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html).
