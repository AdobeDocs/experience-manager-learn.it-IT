---
title: Esportare frammenti esperienza in Adobe Target
description: Scopri come pubblicare ed esportare Frammenti di esperienza AEM come Offerte Adobe Target.
feature: Experience Fragments
version: Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '180'
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

Se si esporta un frammento di esperienza in Adobe Target senza le autorizzazioni corrette in Adobe Admin Console, si verifica il seguente errore nel servizio di authoring dell’AEM:

![Errore nell&#39;interfaccia utente API di Target](assets/error-target-offer.png)

... e i seguenti messaggi di registro nel registro `aemerror`:

![Errore console API di Target](assets/target-console-error.png)

#### Risoluzione

1. Accedi a [Admin Console](https://adminconsole.adobe.com/) con diritti di amministratore per il profilo di prodotto Adobe Target utilizzato ma con integrazione AEM
2. Seleziona __Prodotti > Adobe Target > Profilo prodotto__
3. Nella scheda __Integrazioni__, seleziona l&#39;integrazione per il tuo ambiente AEM as a Cloud Service (con lo stesso nome del progetto Adobe Developer).
4. Assegna la mansione __Editor__ o __Approvatore__

   ![Errore API di Target](assets/target-permissions.png)

L’aggiunta delle autorizzazioni corrette all’integrazione Adobe Target dovrebbe risolvere questo errore.

## Collegamenti di supporto

+ [Debugger Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
