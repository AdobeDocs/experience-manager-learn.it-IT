---
title: Salvare e riprendere le lettere
seo-title: Save and resume letters
description: Scopri come salvare e recuperare le bozze di lettere
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introduzione

Le comunicazioni interattive consentono agli agenti di preparare corrispondenze ad-hoc per salvare le corrispondenze parzialmente completate e recuperarle per continuare a funzionare. AEM Forms fornisce [Interfaccia Service Provider](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Il cliente deve implementare questa interfaccia per ottenere la funzionalità Save and Resume .

Questo articolo utilizza il database MySQL per memorizzare i metadati dell&#39;istanza della lettera. I dati della lettera vengono memorizzati nel file system.

Il video seguente illustra il caso d’uso:

>[!VIDEO](https://video.tv.adobe.com/v/342129/quality=9)

## Prerequisiti

Per implementare la soluzione in modo da soddisfare le tue esigenze è necessario quanto segue

* Esperienza di lavoro con AEM Forms
* AEM Server 6.5 con Forms Add on
* Deve essere familiare nella creazione di bundle OSGI