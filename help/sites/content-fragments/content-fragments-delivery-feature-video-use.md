---
title: Distribuzione di frammenti di contenuto in AEM
seo-title: Distribuzione di frammenti di contenuto in Adobe Experience Manager
description: I frammenti di contenuto, indipendenti dal layout, possono essere utilizzati direttamente in  AEM Sites con i componenti core oppure possono essere distribuiti in modo headless ai canali a valle.
seo-description: I frammenti di contenuto, indipendenti dal layout, possono essere utilizzati direttamente in  AEM Sites con i componenti core oppure possono essere distribuiti in modo headless ai canali a valle.
sub-product: content-services
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 6%

---


# Distribuzione di frammenti di contenuto {#delivering-content-fragments}

Adobe Experience Manager (AEM) I frammenti di contenuto sono contenuti editoriali basati su testo che possono includere alcuni elementi dati strutturati associati ma considerati contenuti puri senza informazioni sulla progettazione o sul layout. I frammenti di contenuto vengono generalmente creati come contenuti che non riguardano i canali, destinati ad essere utilizzati e riutilizzati tra i canali, che a loro volta racchiudono il contenuto in un&#39;esperienza specifica per il contesto.

I frammenti di contenuto, indipendenti dal layout, possono essere utilizzati direttamente in  AEM Sites con i componenti core oppure possono essere distribuiti in modo headless ai canali a valle.

Questa serie video illustra le opzioni di distribuzione per l’utilizzo dei frammenti di contenuto. Informazioni dettagliate sulla definizione e la creazione di frammenti di contenuto sono disponibili qui[.](content-fragments-feature-video-use.md)

1. Utilizzo di frammenti di contenuto nelle pagine Web
2. Esposizione di frammenti di contenuto come JSON tramite AEM Content Services
3. Utilizzo dell’API HTTP Assets

## Utilizzo di frammenti di contenuto nelle pagine Web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

I frammenti di contenuto possono essere utilizzati  pagine AEM Sites, o in modo simile, in frammenti esperienza, utilizzando il componente [Frammento di contenuto ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) dei componenti core di AEM WCM.

I componenti frammento di contenuto possono essere formattati utilizzando AEM Sistema di stile per visualizzare il contenuto come necessario.

## Esposizione di frammenti di contenuto come JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services semplifica la creazione di AEM endpoint HTTP basati su pagina che eseguono il rendering del contenuto in un formato JSON normalizzato.

Il video precedente utilizza il [componente Frammento di contenuto](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) per esporre singoli frammenti di contenuto. Il [componente Elenco frammenti di contenuto](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) è un nuovo componente che consente all&#39;autore di definire una query per la compilazione dinamica della pagina con un elenco di frammenti di contenuto. Il componente Elenco frammenti di contenuto è preferibile se è necessario esporre più frammenti di contenuto.

*Esempio di payload JSON end point di Content Services:*\
**[atleti.json](assets/athletes.json)**

## Utilizzo dell’API HTTP Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

La prima è stata introdotta nella AEM 6.5, con il supporto migliorato per i frammenti di contenuto con l&#39;API HTTP Assets. Questo fornisce agli sviluppatori un modo semplice per eseguire operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) rispetto ai frammenti di contenuto.

*Richieste POSTMAN di esempio:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Quale metodo di consegna utilizzare

### Canale web

L’approccio alla distribuzione di un frammento di contenuto tramite un canale Web è semplice utilizzando il componente Frammento di contenuto con  AEM Sites.

### Senza testa

Esistono due opzioni per esporre il frammento di contenuto come JSON per supportare un canale di terze parti in un caso di utilizzo headless:

1. Utilizzate AEM pagine di servizi di contenuto e API proxy (video 2) quando il caso d&#39;uso principale è la distribuzione di frammenti di contenuto da utilizzare (sola lettura) tramite un canale di terze parti. Il framework Content Services offre maggiore flessibilità e opzioni su quali dati vengono esposti. Gli sviluppatori possono anche estendere il framework Content Services per migliorare e/o arricchire i dati.

2. Utilizzate l&#39;API HTTP Assets (video 3) quando il canale di terze parti deve modificare e/o aggiornare i frammenti di contenuto. Un caso tipico è l’assimilazione di contenuti di terze parti in un ambiente di authoring AEM.

## Risorse aggiuntive {#additional-resources}

* [Creazione di frammenti di contenuto](content-fragments-feature-video-use.md)
* [AEM componenti core WCM](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html)
* [AEM componente frammento di contenuto principale di WCM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

Per scaricare e installare il pacchetto di seguito in un’istanza AEM 6.4+ per lo stato finale dalla serie video:\
**[aem_demo_fluid-experience-content-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
