---
title: Esportare frammenti esperienza in Adobe Target
description: Scopri come pubblicare ed esportare Frammenti di esperienza AEM come Offerte Adobe Target.
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
ht-degree: 3%

---

# Esporta frammento esperienza in Adobe Target {#experience-fragment-target}

Scopri come esportare Frammenti di esperienza AEM come offerte Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Passaggi successivi

+ [Creare un’attività Target tramite le offerte dei frammenti di esperienza](./create-target-activity.md)

## Risoluzione dei problemi

### Esportazione di frammenti di esperienza in Target non riuscita

#### Errore

Se si esporta un frammento di esperienza in Adobe Target senza le autorizzazioni corrette in Adobe Admin Console, si verifica il seguente errore nel servizio di authoring di AEM:

    ![Errore nell’interfaccia utente dell’API di Target](assets/error-target-offer.png)

... e i seguenti messaggi di registro in `aemerror` registro:

    ![Errore della console API di Target](assets/target-console-error.png)

#### Risoluzione

1. Accedi a [Admin Console](https://adminconsole.adobe.com/) con diritti di amministratore per il profilo di prodotto Adobe Target utilizzato, ma con integrazione AEM
2. Seleziona __Prodotti > Adobe Target > Profilo prodotto__
3. Sotto __Integrazioni__ , seleziona l’integrazione per l’ambiente AEM as a Cloud Service (con lo stesso nome del progetto Adobe I/O)
4. Assegna __Editor__ o __Approvatore__ ruolo

   ![Errore API di Target](assets/target-permissions.png)

L’aggiunta delle autorizzazioni corrette all’integrazione Adobe Target dovrebbe risolvere questo errore.

## Collegamenti di supporto

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
