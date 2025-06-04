---
title: Integrazione dei tag in Adobe Experience Platform e AEM
description: I tag nella raccolta dati di Experience Platform sono la soluzione di gestione dei tag di nuova generazione di Adobe e il modo migliore per distribuire Adobe Analytics, Target, Audience Manager e molte altre soluzioni. Ottieni una panoramica dei tag in Adobe Experience Platform e dell’integrazione consigliata con Adobe Experience Manager.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Integrazione dei tag nella raccolta dati di Experience Platform e AEM {#overview}

Scopri come integrare i tag in Adobe Experience Platform con Adobe Experience Manager.

I tag sono la tecnologia di nuova generazione di Adobe Experience Platform per la gestione dei tag. I tag forniscono il modo più semplice per distribuire Adobe Analytics, Target, Audience Manager e molte altre soluzioni. Ottieni una panoramica dei tag e dell’integrazione consigliata con Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3445208?quality=12&learn=on&captions=ita)

## Prerequisiti

Per l’integrazione dei tag nella raccolta dati di Experience Platform sono necessari i seguenti elementi.

+ Accesso come amministratore AEM all’ambiente AEM as a Cloud Service
+ Un sito di riferimento come [WKND](https://github.com/adobe/aem-guides-wknd) distribuito in esso.
+ Accesso alla soluzione di raccolta dati di Adobe Experience Platform
+ Accesso come amministratore di sistema ad [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## Passaggi di alto livello

+ Nella raccolta dati di Adobe Experience Platform, crea una proprietà tag e modificala in _Aggiungi regola_. Quindi _Aggiungi libreria_, seleziona la regola appena aggiunta, approvala e pubblicala.
+ Connetti AEM e i tag utilizzando una configurazione IMS esistente (o nuova)
+ In AEM, crea una configurazione del servizio cloud di tag, quindi applicala a un sito esistente e verifica infine che la proprietà tag e le relative librerie siano caricate sul sito Pubblicato o di Authoring.

## Passaggi successivi

[Creare una proprietà tag](create-tag-property.md)

## Risorse aggiuntive {#additional-resources}

+ [Integrazioni di Experience Platform con le applicazioni Experience Cloud](https://experienceleague.adobe.com/it/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications)
+ [Panoramica sui tag](https://experienceleague.adobe.com/it/docs/experience-platform/tags/home)
+ [Implementazione di Experience Cloud nei siti web con i tag](https://experienceleague.adobe.com/it/docs/platform-learn/implement-in-websites/overview)
