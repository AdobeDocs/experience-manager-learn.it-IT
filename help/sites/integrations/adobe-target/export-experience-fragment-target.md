---
title: Esporta Frammenti esperienza in Adobe Target
description: Scopri come pubblicare ed esportare AEM Frammento esperienza come offerte Adobe Target.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 5%

---

# Esportare i frammenti esperienza in Adobe Target {#experience-fragment-target}

Scopri come esportare AEM Frammento esperienza come offerte Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Passaggi successivi

+ [Creare un’attività Target utilizzando le offerte dei frammenti esperienza](./create-target-activity.md)

## Risoluzione dei problemi

### Esportazione di frammenti di esperienza in Target non riuscita

#### Errore

Quando si esporta un frammento esperienza in Adobe Target senza le autorizzazioni corrette in Adobe Admin Console, si verifica il seguente errore sul servizio AEM Author:

    ![Errore interfaccia utente API di Target](assets/error-target-offer.png)

... e i seguenti messaggi di log nel registro `aemerror`:

    ![Errore della console API di Target](assets/target-console-error.png)

#### Risoluzione

1. Accedi a [Admin Console](https://adminconsole.adobe.com/) con diritti amministrativi per il profilo di prodotto Adobe Target utilizzato, ma con integrazione AEM
2. Seleziona __Prodotti > Adobe Target > Profilo prodotto__
3. Nella scheda __Integrazioni__ , seleziona l’integrazione per il tuo AEM come ambiente di Cloud Service (stesso nome del progetto Adobe I/O)
4. Assegna il ruolo __Editor__ o __Approvatore__

   ![Errore API di Target](assets/target-permissions.png)

L’aggiunta dell’autorizzazione corretta all’integrazione di Adobe Target dovrebbe risolvere questo errore.

## Collegamenti di supporto

+ [Debugger Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Debugger Adobe Experience Cloud - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
