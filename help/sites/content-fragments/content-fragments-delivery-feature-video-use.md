---
title: Distribuzione di frammenti di contenuto nell’AEM
description: I frammenti di contenuto, indipendentemente dal layout, possono essere utilizzati direttamente in AEM Sites con i componenti core o consegnati in modo headless ai canali a valle.
feature: Content Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 906
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---

# Distribuzione di frammenti di contenuto {#delivering-content-fragments}

I frammenti di contenuto di Adobe Experience Manager (AEM) sono contenuti editoriali basati su testo che possono includere alcuni elementi di dati strutturati associati ma considerati contenuti puri senza informazioni di progettazione o layout. I frammenti di contenuto vengono generalmente creati come contenuti indipendenti dal canale, destinati a essere utilizzati e riutilizzati su tutti i canali, che a loro volta racchiudono il contenuto in un’esperienza specifica del contesto.

I frammenti di contenuto, indipendentemente dal layout, possono essere utilizzati direttamente in AEM Sites con i componenti core o consegnati in modo headless ai canali a valle.

Questa serie di video illustra le opzioni di consegna per l’utilizzo dei frammenti di contenuto. Dettagli sulla definizione di e [authoring dei frammenti di contenuto sono disponibili qui](content-fragments-feature-video-use.md).

1. Utilizzo dei frammenti di contenuto nelle pagine web
2. Esposizione di frammenti di contenuto come JSON con AEM Content Services
3. Utilizzo dell’API HTTP delle risorse

## Utilizzo di frammenti di contenuto nelle pagine web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

I Frammenti di contenuto possono essere utilizzati sulle pagine di AEM Sites o in modo simile, Frammenti di esperienza, utilizzando i Componenti core WCM dell’AEM. [Componente Frammento di contenuto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it).

Lo stile dei componenti dei frammenti di contenuto può essere impostato utilizzando il sistema di stili dell’AEM per visualizzare il contenuto in base alle esigenze.

## Esposizione di frammenti di contenuto come JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services semplifica la creazione di endpoint HTTP basati su pagina AEM per il rendering del contenuto in formato JSON normalizzato.

Il video precedente utilizza [Componente Frammento di contenuto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it) per esporre singoli frammenti di contenuto. Il [Componente Elenco frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) è un nuovo componente che consente all’autore di definire una query per compilare in modo dinamico la pagina con un elenco di Frammenti di contenuto. Il componente Elenco frammenti di contenuto è da preferirsi quando è necessario esporre più frammenti di contenuto.

*Esempio di payload JSON dell’endpoint di Content Services:*\
**[atletes.json](assets/athletes.json)**

## Utilizzo dell’API HTTP delle risorse

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

Introdotto per la prima volta in AEM 6.5, è stato migliorato il supporto dei frammenti di contenuto con l’API HTTP delle risorse. Questo offre agli sviluppatori un modo semplice per eseguire operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) sui frammenti di contenuto.

*Esempio di richieste POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Quale metodo di consegna utilizzare

### Canale web

L’approccio per la distribuzione di un frammento di contenuto tramite un canale web è semplice utilizzando il componente Frammento di contenuto con AEM Sites.

### Headless

Esistono due opzioni per esporre Frammento di contenuto come JSON per supportare un canale di terze parti in un caso d’uso headless:

1. Utilizza le pagine AEM Content Services e Proxy API (#2 video) quando il caso d’uso principale è la distribuzione di frammenti di contenuto da consumare (sola lettura) da un canale di terze parti. Il framework Content Services offre maggiore flessibilità e opzioni per quanto riguarda i dati che vengono esposti. Gli sviluppatori possono anche estendere il framework Content Services per aumentare e/o arricchire i dati.

2. Utilizza l’API HTTP delle risorse (#3 video) quando il canale di terze parti deve modificare e/o aggiornare frammenti di contenuto. Un caso d’uso tipico è l’acquisizione di contenuti di terze parti in un ambiente di authoring AEM.

## Risorse aggiuntive {#additional-resources}

* [Authoring dei frammenti di contenuto](content-fragments-feature-video-use.md)
* [Componenti core WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [Componente frammento di contenuto principale WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it)

Per scaricare e installare il pacchetto seguente su un’istanza AEM 6.4+ per lo stato finale dalla serie video:\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
