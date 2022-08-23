---
title: Distribuzione di frammenti di contenuto in AEM
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: I frammenti di contenuto, indipendentemente dal layout, possono essere utilizzati direttamente in AEM Sites con i componenti core o possono essere consegnati in modo headless ai canali a valle.
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 5%

---

# Distribuzione di frammenti di contenuto {#delivering-content-fragments}

Adobe Experience Manager (AEM) I frammenti di contenuto sono contenuti editoriali basati su testo che possono includere alcuni elementi di dati strutturati associati, ma considerati contenuti puri senza informazioni di progettazione o layout. I frammenti di contenuto vengono generalmente creati come contenuti indipendenti dai canali, destinati ad essere utilizzati e riutilizzati su più canali, che a loro volta racchiudono il contenuto in un’esperienza specifica per il contesto.

I frammenti di contenuto, indipendentemente dal layout, possono essere utilizzati direttamente in AEM Sites con i componenti core o possono essere consegnati in modo headless ai canali a valle.

Questa serie video illustra le opzioni di consegna per l’utilizzo dei frammenti di contenuto. Dettagli su definizione e [authoring I frammenti di contenuto sono disponibili qui](content-fragments-feature-video-use.md).

1. Utilizzo di frammenti di contenuto nelle pagine web
2. Esposizione di frammenti di contenuto come JSON tramite AEM Content Services
3. Utilizzo dell’API HTTP Assets

## Utilizzo di frammenti di contenuto nelle pagine web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

I frammenti di contenuto possono essere utilizzati nelle pagine AEM Sites o in modo simile, nei frammenti di esperienza, utilizzando i componenti core di AEM WCM [Componente Frammento di contenuto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it).

I componenti Frammento di contenuto possono essere formattati utilizzando AEM sistema di stili per visualizzare il contenuto in base alle esigenze.

## Esposizione di frammenti di contenuto come JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services facilita la creazione di AEM endpoint HTTP basati su pagina che eseguono il rendering del contenuto in un formato JSON normalizzato.

Il video di cui sopra utilizza il [Componente Frammento di contenuto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) per esporre singoli frammenti di contenuto. La [Componente Elenco frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) è un nuovo componente che consente all’autore di definire una query che popolerà in modo dinamico la pagina con un elenco di frammenti di contenuto. Il componente Elenco frammenti di contenuto è preferibile quando è necessario esporre più frammenti di contenuto.

*Esempio di payload JSON end-point di Content Services:*\
**[atleti.json](assets/athletes.json)**

## Utilizzo dell’API HTTP Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

Introdotto in AEM 6.5, viene migliorato il supporto per i frammenti di contenuto con l’API HTTP Assets. Questo fornisce agli sviluppatori un modo semplice per eseguire operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) rispetto ai frammenti di contenuto.

*Esempio di richieste POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Quale metodo di consegna utilizzare

### Canale web

L’approccio alla distribuzione di un frammento di contenuto tramite un canale web è semplice utilizzando il componente Frammento di contenuto con AEM Sites.

### Headless

Esistono due opzioni per esporre Frammento di contenuto come JSON per supportare un canale di terze parti in un caso d’uso headless:

1. Utilizza AEM Content Services e le pagine API proxy (video 2) quando il caso d’uso principale è quello di distribuire frammenti di contenuto destinati all’uso (sola lettura) da un canale di terze parti. Il framework Content Services offre maggiore flessibilità e opzioni per determinare quali dati vengono esposti. Gli sviluppatori possono inoltre estendere il framework Content Services per integrare e/o arricchire i dati.

2. Utilizza l’API HTTP delle risorse (video 3) quando il canale di terze parti deve modificare e/o aggiornare i frammenti di contenuto. Un caso d’uso tipico è l’acquisizione di contenuti di terze parti in un ambiente di authoring AEM.

## Risorse aggiuntive {#additional-resources}

* [Authoring di frammenti di contenuto](content-fragments-feature-video-use.md)
* [AEM componenti core WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [AEM componente Frammento di contenuto core WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

Per scaricare e installare il pacchetto qui sotto su un&#39;istanza AEM 6.4+ per lo stato finale dalla serie video:\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
