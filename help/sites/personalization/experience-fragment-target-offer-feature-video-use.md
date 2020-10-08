---
title: Utilizzo AEM offerte di frammenti esperienza in  Adobe Target
seo-title: Utilizzo AEM offerte di frammenti esperienza in  Adobe Target
description: Adobe Experience Manager 6.4 rivoluziona il flusso di lavoro di personalizzazione tra AEM e Target. Le esperienze create all'interno di AEM ora possono essere distribuite direttamente  Adobe Target come offerte HTML. Consente agli esperti di marketing di sottoporre a test e personalizzare i contenuti tra canali diversi.
seo-description: Adobe Experience Manager 6.4 rivoluziona il flusso di lavoro di personalizzazione tra AEM e Target. Le esperienze create all'interno di AEM ora possono essere distribuite direttamente  Adobe Target come offerte HTML. Consente agli esperti di marketing di sottoporre a test e personalizzare i contenuti tra canali diversi.
sub-product: content-services
feature: experience-fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 11%

---


# Utilizzo di offerte di frammenti esperienza in  Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 rivoluziona il flusso di lavoro di personalizzazione tra AEM e Target. Le esperienze create all&#39;interno di AEM ora possono essere distribuite direttamente  Adobe Target come offerte HTML. Consente agli esperti di marketing di sottoporre a test e personalizzare i contenuti tra canali diversi.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Consigliato di utilizzare la libreria client at.js e la best practice consiste nell&#39;utilizzare soluzioni di gestione tag come Launch by Adobe,  DTM Adobe o qualsiasi soluzione di gestione tag di terze parti per aggiungere librerie di destinazione alle pagine del sito

>[!NOTE]
>
>AEM Offerte frammenti esperienza in  Adobe Target è disponibile anche come Feature Pack per gli utenti AEM 6.3. Per i Feature Pack e le dipendenze, consulta la sezione riportata di seguito.


* Adobe Experience Manager, che consente di creare contenuti di facile utilizzo e potenti insieme a  intelligenza artificiale (AI) e apprendimento automatico di Adobe Target, consente agli autori di creare e gestire contenuti per tutti i canali in una posizione centralizzata. Grazie alla possibilità di esportare frammenti esperienza in  Adobe Target come offerte HTML, gli addetti al marketing possono ora creare un&#39;esperienza più personalizzata utilizzando queste offerte e possono ora sottoporre a test e scalare ogni esperienza creata.
* La differenza principale tra le offerte HTML e le offerte di frammenti esperienza è che la modifica per un secondo momento può essere eseguita solo in AEM e quindi sincronizzata con  Adobe Target
* La configurazione del servizio Target Cloud applicata alla cartella dei frammenti esperienza eredita da tutti i frammenti esperienza creati direttamente nella cartella principale. La cartella figlio non eredita la configurazione dei servizi cloud padre.
* Per creare un&#39;offerta personalizzata ora possiamo sfruttare facilmente i contenuti memorizzati in AEM.
* Potete creare tipi di attività Target, comprese le attività con tecnologia Sensei come Auto-Allocazione, Auto Target e  Automated Personalization

## AEM 6.3 Feature Pack e dipendenze {#aem-feature-packs-and-dependencies}

| AEM 6.3 Feature Pack | Dipendenze |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefisspack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefisspack:aem-6.3.2-cfp:2.0 |

## Risorse aggiuntive {#additional-resources}

* [Documentazione sui frammenti esperienza](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Utilizzo di Frammenti esperienza](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
