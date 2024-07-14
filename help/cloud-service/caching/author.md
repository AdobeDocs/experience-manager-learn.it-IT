---
title: Memorizzazione in cache del servizio di authoring AEM
description: Panoramica generale sul caching del servizio AEM as a Cloud Service Author.
version: Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: b8e09820-f1f2-4897-b454-16c0df5a0459
duration: 56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---

# Autore AEM

La funzione di caching è limitata per gli autori AEM, a causa della natura altamente dinamica e sensibile alle autorizzazioni dei contenuti che forniscono. In generale, si sconsiglia di personalizzare la memorizzazione in cache per l’istanza di AEM Author e di fare affidamento sulle configurazioni di cache fornite da Adobe per garantire un’esperienza performante.

![Diagramma panoramica sul caching dell&#39;autore AEM](./assets/author/author-all.png){align="center"}

Sebbene sia sconsigliato personalizzare la memorizzazione in cache in AEM Author, è utile comprendere che AEM Author ha una rete CDN gestita da Adobe, ma non dispone di AEM Dispatcher. Ricorda che tutte le configurazioni di AEM Dispatcher vengono ignorate nell’istanza di authoring dell’AEM, in quanto non dispone di un Dispatcher AEM.

## CDN

Il servizio di authoring AEM utilizza una rete CDN, tuttavia il suo scopo è quello di migliorare la distribuzione delle risorse di prodotto e non deve essere configurato in modo esteso, consentendo al contrario di funzionare così com’è.

![Diagramma panoramica sulla memorizzazione nella cache di Publish per AEM](./assets/author/author-cdn.png){align="center"}

La rete CDN di authoring dell’AEM è posizionata tra l’utente finale, in genere un addetto al marketing o un autore di contenuti, e l’autore dell’AEM. Memorizza nella cache i file immutabili, ad esempio le risorse statiche che alimentano l’esperienza di authoring dell’AEM, e non i contenuti creati.

Il CDN dell&#39;autore AEM memorizza nella cache diversi tipi di risorse che potrebbero essere di interesse, tra cui un TTL [personalizzabile su query persistenti](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) e un TTL [lungo su librerie client personalizzate](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### Durata predefinita della cache

Le seguenti risorse rivolte al cliente sono memorizzate nella cache dalla rete CDN di creazione AEM e hanno la seguente durata predefinita della cache:

| Tipo di contenuto | Durata predefinita cache CDN |
|:------------ |:---------- |
| [Query persistenti (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1 minuto |
| [Librerie client (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 giorni |
| [Tutto il resto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Non memorizzato in cache |


## Dispatcher AEM

Il servizio di authoring AEM non include il Dispatcher AEM e utilizza solo la [CDN](#cdn) per il caching.
