---
title: Integrazione di tag in Adobe Experience Platform e AEM
description: I tag in Raccolta dati di Experienci Platform sono la soluzione di gestione dei tag di nuova generazione di Adobe e il modo migliore per distribuire Adobe Analytics, Target, Audienci Manager e molte altre soluzioni. Panoramica dei tag in Adobe Experience Platform e dell’integrazione consigliata con Adobe Experience Manager.
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
duration: 256
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 2%

---

# Integrazione dei tag di raccolta dati Experienci Platform e AEM {#overview}

Scopri come integrare i tag in Adobe Experience Platform con Adobe Experience Manager.

I tag sono la tecnologia di nuova generazione di Adobe Experience Platform per la gestione dei tag. I tag forniscono il modo più semplice per distribuire Adobe Analytics, Target, Audienci Manager e molte altre soluzioni. Ottieni una panoramica di Tags e dell’integrazione consigliata con Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## Prerequisiti

Per l’integrazione dei tag di raccolta dati Experienci Platform sono necessari i seguenti elementi.

+ Accesso dell’amministratore AEM all’ambiente as a Cloud Service AEM
+ Un sito di riferimento come [WKND](https://github.com/adobe/aem-guides-wknd) implementato su di esso.
+ Accesso alla soluzione di raccolta dati Adobe Experience Platform
+ Accesso amministratore di sistema a [Console Adobe Developer](https://developer.adobe.com/developer-console/)


## Passaggi di alto livello

+ In Raccolta dati di Adobe Experience Platform, crea una proprietà Tag e modificala in _Aggiungi regola_. Then _Aggiungi libreria_, seleziona la regola appena aggiunta, la approva e la pubblica.
+ Connettere AEM e tag utilizzando la configurazione IMS esistente (o nuova)
+ In AEM, crea una configurazione di servizio cloud di tag, quindi applicala a un sito esistente e verifica infine che la proprietà Tags e le relative librerie siano caricate sul sito Published o Author.

## Passaggi successivi

[Creare una proprietà tag](create-tag-property.md)

## Risorse aggiuntive {#additional-resources}

+ [Integrazioni Experienci Platform con applicazioni Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Panoramica sui tag](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementazione dell’Experience Cloud nei siti web con i tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
