---
title: Crea  account di Cloud Service Adobe Target
description: Procedura dettagliata sull'integrazione di Adobe Experience Manager come Cloud Service con  Adobe Target mediante l'autenticazione Cloud Service e  Adobe IMS
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Crea  account di Cloud Service Adobe Target {#adobe-target-cloud-service}

Procedura dettagliata sull&#39;integrazione di Adobe Experience Manager come Cloud Service con  Adobe Target mediante l&#39;autenticazione IMS di Cloud Service e  Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Esiste un problema noto  configurazione dei Cloud Services Adobe Target mostrato nel video. È invece consigliabile seguire gli stessi passaggi nel video, ma utilizzare la configurazione [Cloud Services](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)precedenti  Adobe Target.

## Problemi comuni

Quando si esporta un frammento esperienza in  destinazione Adobe, se l&#39;integrazione di Target nella console di amministrazione non dispone dell&#39;autorizzazione appropriata, potrebbe essere visualizzato un errore come mostrato di seguito.

**Errore** interfaccia utente API![Target errore interfaccia utente](assets/error-target-offer.png)

**Errore** di registro della console API![Target](assets/target-console-error.png)


**Soluzione**

1. Passa a [Admin Console](https://adminconsole.adobe.com/)
2. Selezionate Products (Prodotti) >  Adobe Target > Product Profile (Profilo di prodotto)
3. Nella scheda Integrazioni, seleziona il progetto di integrazione ( progetto I/O del Adobe)
4. Assegnare il ruolo di Editor o Approver.

![Errore API di Target](assets/target-permissions.png)

L&#39;aggiunta dell&#39;autorizzazione giusta all&#39;integrazione con Target ti aiuterà a risolvere il problema di cui sopra.