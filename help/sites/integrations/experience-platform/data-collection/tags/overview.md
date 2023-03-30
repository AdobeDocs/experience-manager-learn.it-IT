---
title: Integrazione dei tag di raccolta dati di Experience Platform (Launch) e dei AEM
description: I tag nella raccolta dati di Experience Platform sono la soluzione di gestione tag di nuova generazione di Adobe e il modo migliore per distribuire Adobe Analytics, Target, Audience Manager e molte altre soluzioni. Ottieni una panoramica dei tag (precedentemente noti come Launch) e dell’integrazione consigliata con Adobe Experience Manager.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# Integrazione dei tag e dei AEM di raccolta dati di Experience Platform {#overview}

Scopri come integrare l’Experience Platform _Tag per la raccolta dati_ (precedentemente noto come Launch) con Adobe Experience Manager.

>[!NOTE]
>
>Adobe Experience Platform Launch è stato classificato come una suite di tecnologie di raccolta dati in Adobe Experience Platform. Di conseguenza, sono state introdotte diverse modifiche terminologiche nella documentazione del prodotto. Consulta quanto segue [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) per un riferimento consolidato delle modifiche terminologiche.


I tag sono la tecnologia di nuova generazione di Adobe Experience Platform per la gestione dei tag. I tag forniscono il modo più semplice per implementare Adobe Analytics, Target, Audience Manager e molte altre soluzioni. Panoramica dei tag e dell’integrazione consigliata con Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## Prerequisiti

Quando si integrano i tag di raccolta dati di Experience Platform, sono necessari i seguenti elementi:

+ Accesso AEM amministratore all’ambiente as a Cloud Service AEM
+ Un sito di riferimento come [WKND](https://github.com/adobe/aem-guides-wknd) implementato su di esso.
+ Accesso alla soluzione Adobe Experience Platform Data Collection
+ Accesso dell&#39;amministratore di sistema a [Console Adobe Developer](https://developer.adobe.com/developer-console/)


## Fasi di alto livello

+ In Raccolta dati di Adobe Experience Platform, crea una proprietà Tag e modificala in _Aggiungi regola_. Then _Aggiungi libreria_, seleziona la regola appena aggiunta, approvala e pubblicala.
+ Collegare AEM e tag utilizzando la configurazione IMS esistente (o nuova)
+ In AEM, crea una configurazione di Launch Cloud Services, quindi applicala a un sito esistente e infine verifica che la proprietà Tags e le relative librerie siano caricate sul sito Pubblicato o Autore.

## Passaggi successivi

[Creare una proprietà tag](create-tag-property.md)

## Risorse aggiuntive {#additional-resources}

+ [Integrazioni di Experience Platform con le applicazioni Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Panoramica sui tag](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementazione dell’Experience Cloud nei siti web con tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
