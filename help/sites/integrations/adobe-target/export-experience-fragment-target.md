---
title: Esporta Frammenti esperienza in Adobe Target
description: Scopri come pubblicare ed esportare Frammenti esperienza AEM come offerte di Adobe Target.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 6%

---


# Esportare un frammento esperienza in Adobe Target {#experience-fragment-target}

Scopri come esportare i frammenti esperienza AEM come offerte Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Passaggi successivi

+ [Creare un’attività Target utilizzando le offerte dei frammenti esperienza](./create-target-activity.md)

## Risoluzione dei problemi

### Esportazione di frammenti di esperienza in Target non riuscita

#### Errore

L’esportazione di frammenti di esperienza in Adobe Target senza le autorizzazioni corrette in Adobe Admin Console genera il seguente errore sul servizio AEM Author:

    ![Errore interfaccia utente API di Target](assets/error-target-offer.png)

... e i seguenti messaggi di log nel registro `aemerror`:

    ![Errore della console API di Target](assets/target-console-error.png)

#### Risoluzione

1. Accedi a [Admin Console](https://adminconsole.adobe.com/) con i diritti amministrativi per il profilo di prodotto Adobe Target utilizzato ma con l’integrazione AEM
2. Seleziona __Prodotti > Adobe Target > Profilo prodotto__
3. Alla voce __Integrazioni__ , seleziona l’integrazione per l’ambiente AEM as a Cloud Service (stesso nome del progetto Adobe I/O)
4. Assegna il ruolo __Editor__ o __Approvatore__

   ![Errore API di Target](assets/target-permissions.png)

L&#39;aggiunta dell&#39;autorizzazione corretta all&#39;integrazione di Adobe Target dovrebbe risolvere questo errore.

## Collegamenti di supporto

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)