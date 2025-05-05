---
title: Salva e riprendi lettere
description: Scopri come salvare e recuperare le bozze di lettere
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 160
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Introduzione

Le comunicazioni interattive consentono agli agenti che preparano corrispondenze ad hoc di salvare le corrispondenze parzialmente completate e di recuperare le stesse per continuare a lavorare. AEM Forms fornisce [Interfaccia provider di servizi](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Il cliente deve implementare questa interfaccia per ottenere la funzionalità Salva e riprendi.

In questo articolo viene utilizzato il database MySQL per memorizzare i metadati dell&#39;istanza della lettera. I dati della lettera vengono memorizzati nel file system.

Il video seguente illustra il caso d’uso:

>[!VIDEO](https://video.tv.adobe.com/v/3441448?quality=12&learn=on&captions=ita)

## Prerequisiti

Per implementare la soluzione in base alle tue esigenze, dovrai disporre dei seguenti elementi

* Esperienza di lavoro con AEM Forms
* AEM Server 6.5 con componente aggiuntivo Forms
* Deve avere familiarità con la creazione di bundle OSGI
