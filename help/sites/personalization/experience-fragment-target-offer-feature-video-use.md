---
title: Utilizzo AEM offerte di frammenti esperienza in Adobe Target
seo-title: Utilizzo AEM offerte di frammenti esperienza in Adobe Target
description: Adobe Experience Manager 6.4 riimmagina il flusso di lavoro di personalizzazione tra AEM e Target. Le esperienze create in AEM ora possono essere distribuite direttamente in Adobe Target as HTML Offers. Consente agli addetti al marketing di testare e personalizzare facilmente i contenuti tra diversi canali.
seo-description: Adobe Experience Manager 6.4 riimmagina il flusso di lavoro di personalizzazione tra AEM e Target. Le esperienze create in AEM ora possono essere distribuite direttamente in Adobe Target as HTML Offers. Consente agli addetti al marketing di testare e personalizzare facilmente i contenuti tra diversi canali.
sub-product: content-services
feature: Frammenti di esperienza
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: Personalizzazione
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 11%

---


# Utilizzo di offerte di frammenti esperienza in Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 riimmagina il flusso di lavoro di personalizzazione tra AEM e Target. Le esperienze create in AEM ora possono essere distribuite direttamente in Adobe Target as HTML Offers. Consente agli addetti al marketing di testare e personalizzare facilmente i contenuti tra diversi canali.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Consigliato di utilizzare la libreria client at.js e la best practice consiste nell’utilizzare soluzioni di gestione tag come Launch by Adobe, Adobe DTM o qualsiasi soluzione di gestione tag di terze parti per aggiungere librerie di target alle pagine del sito

>[!NOTE]
>
>AEM offerte per frammenti esperienza in Adobe Target è disponibile anche come Feature Pack per gli utenti AEM 6.3. Vedi la sezione sottostante per i Feature Pack e le dipendenze.


* Il meccanismo di creazione dei contenuti semplice e potente di Adobe Experience Manager, unitamente all’intelligenza artificiale (AI) e all’apprendimento automatico di Adobe Target, consente agli autori dei contenuti di creare e gestire i contenuti per tutti i canali in una posizione centralizzata. Con la possibilità di esportare frammenti di esperienza in Adobe Target come offerte HTML, gli addetti al marketing hanno ora maggiore flessibilità per creare un’esperienza più personalizzata utilizzando queste offerte e possono ora testare e scalare ogni esperienza creata.
* La differenza principale tra le offerte HTML e le offerte dei frammenti esperienza è che la modifica per un momento successivo può essere eseguita solo in AEM e quindi sincronizzata con Adobe Target
* La configurazione del servizio Cloud di Target applicata alla cartella Frammenti esperienza eredita tutti i frammenti esperienza creati direttamente nella cartella principale. La cartella figlio non eredita la configurazione di servizi cloud padre.
* Per creare un’offerta personalizzata ora possiamo sfruttare facilmente il contenuto memorizzato in AEM.
* Puoi creare tipi di attività Target, comprese le attività basate su Sensei come Allocazione automatica, Targeting automatico e Automated Personalization

## AEM 6.3 Feature Pack e dipendenze {#aem-feature-packs-and-dependencies}

| Pacchetto di funzioni di AEM 6.3 | Dipendenze |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Risorse aggiuntive {#additional-resources}

* [Documentazione sui frammenti esperienza](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Utilizzo di Frammenti esperienza](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
