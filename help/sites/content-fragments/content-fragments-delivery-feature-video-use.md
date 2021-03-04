---
title: Distribuzione di frammenti di contenuto in AEM
seo-title: Distribuzione di frammenti di contenuto in Adobe Experience Manager
description: I frammenti di contenuto, indipendentemente dal layout, possono essere utilizzati direttamente in AEM Sites con i componenti core o possono essere consegnati in modo headless ai canali a valle.
seo-description: I frammenti di contenuto, indipendentemente dal layout, possono essere utilizzati direttamente in AEM Sites con i componenti core o possono essere consegnati in modo headless ai canali a valle.
sub-product: content-services
feature: Frammenti di contenuto
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Gestione dei contenuti
role: Professionista
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---


# Distribuzione di frammenti di contenuto {#delivering-content-fragments}

I frammenti di contenuto di Adobe Experience Manager (AEM) sono contenuti editoriali basati su testo che possono includere alcuni elementi di dati strutturati associati ma considerati contenuti puri senza informazioni di progettazione o layout. I frammenti di contenuto vengono generalmente creati come contenuti indipendenti dai canali, destinati ad essere utilizzati e riutilizzati su più canali, che a loro volta racchiudono il contenuto in un’esperienza specifica per il contesto.

I frammenti di contenuto, indipendentemente dal layout, possono essere utilizzati direttamente in AEM Sites con i componenti core o possono essere consegnati in modo headless ai canali a valle.

Questa serie video illustra le opzioni di consegna per l’utilizzo dei frammenti di contenuto. I dettagli sulla definizione e la [creazione di frammenti di contenuto sono disponibili qui](content-fragments-feature-video-use.md).

1. Utilizzo di frammenti di contenuto nelle pagine web
2. Esposizione di frammenti di contenuto come JSON tramite AEM Content Services
3. Utilizzo dell’API HTTP Assets

## Utilizzo di frammenti di contenuto nelle pagine Web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

I frammenti di contenuto possono essere utilizzati nelle pagine di AEM Sites o in modo simile, nei frammenti di esperienza, utilizzando il componente [Frammento di contenuto ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) dei componenti core di AEM WCM.

I componenti Frammento di contenuto possono essere formattati utilizzando il sistema di stili di AEM per visualizzare il contenuto in base alle esigenze.

## Esposizione di frammenti di contenuto come JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services facilita la creazione di endpoint HTTP basati su pagina AEM che trasformano i contenuti in un formato JSON normalizzato.

Il video precedente utilizza il [componente Frammento di contenuto](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) per esporre singoli frammenti di contenuto. Il [componente Elenco frammenti di contenuto](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) è un nuovo componente che consente all’autore di definire una query per la compilazione dinamica della pagina con un elenco di frammenti di contenuto. Il componente Elenco frammenti di contenuto è preferibile quando è necessario esporre più frammenti di contenuto.

*Esempio di payload JSON end-point di Content Services:*\
**[atleti.json](assets/athletes.json)**

## Utilizzo dell’API HTTP Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

Introdotto inizialmente in AEM 6.5, offre un supporto migliorato per i frammenti di contenuto con l’API HTTP Assets. Questo fornisce agli sviluppatori un modo semplice per eseguire operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) rispetto ai frammenti di contenuto.

*Esempio di richieste POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Quale metodo di consegna utilizzare

### Canale web

L’approccio alla distribuzione di un frammento di contenuto tramite un canale web è semplice utilizzando il componente Frammento di contenuto con AEM Sites.

### Senza testa

Esistono due opzioni per esporre Frammento di contenuto come JSON per supportare un canale di terze parti in un caso d’uso headless:

1. Utilizza le pagine di AEM Content Services e Proxy API (video 2) quando il caso d’uso principale è distribuire frammenti di contenuto da utilizzare (sola lettura) per un canale di terze parti. Il framework Content Services offre maggiore flessibilità e opzioni per determinare quali dati vengono esposti. Gli sviluppatori possono inoltre estendere il framework Content Services per integrare e/o arricchire i dati.

2. Utilizza l’API HTTP delle risorse (video 3) quando il canale di terze parti deve modificare e/o aggiornare i frammenti di contenuto. Un caso d’uso tipico è l’acquisizione di contenuti di terze parti in un ambiente di authoring AEM.

## Risorse aggiuntive {#additional-resources}

* [Authoring di frammenti di contenuto](content-fragments-feature-video-use.md)
* [Componenti core di AEM WCM](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/introduction.html)
* [Componente frammento di contenuto core WCM AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

Per scaricare e installare il pacchetto qui sotto su un&#39;istanza AEM 6.4+ per lo stato finale dalla serie video:\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
