---
title: Esporta Frammenti esperienza in Adobe Target
description: Scoprite come pubblicare ed esportare AEM frammento esperienza come  offerte Adobe Target.
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 4%

---


# Esporta frammento esperienza in  Adobe Target {#experience-fragment-target}

Scoprite come esportare AEM frammento esperienza come  offerte Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Passaggi successivi

+ [Creazione di un&#39;attività Target tramite Offerte frammenti esperienza](./create-target-activity.md)

## Risoluzione dei problemi

### Esportazione di frammenti esperienza in Target non riuscita

#### Errore

Se si esporta un frammento esperienza in  Adobe Target senza le autorizzazioni corrette in Adobe Admin Console, si verifica il seguente errore nel servizio AEM Author:

    ![Errore interfaccia API di destinazione](assets/error-target-offer.png)

... e i seguenti messaggi di registro nel registro `aemerror`:

    ![Errore della console API di Target](assets/target-console-error.png)

#### Risoluzione

1. Accedete a [ Admin Console](https://adminconsole.adobe.com/) con diritti amministrativi per il profilo di prodotto Adobe Target  utilizzato, ma con l&#39;integrazione AEM
2. Selezionare __Prodotti >  Adobe Target > Profilo prodotto__
3. Nella scheda __Integrations__ (Integrazioni), selezionate l&#39;integrazione per il vostro AEM come ambiente di Cloud Service (stesso nome del progetto  Adobe I/O)
4. Assegna il ruolo __Editor__ o __Approver__

   ![Errore API di Target](assets/target-permissions.png)

Se si aggiunge l&#39;autorizzazione corretta all&#39;integrazione con  Adobe Target, l&#39;errore verrà risolto.

## Collegamenti di supporto

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)