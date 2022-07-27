---
title: Distribuzione di SPA per AEM GraphQL
description: Scopri SPA opzioni di distribuzione per quanto riguarda AEM GraphQL, Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# Distribuzione di un SPA

In questa sezione esamineremo un approccio per la distribuzione di SPA (React, Vue, Angular, ecc.) che richiama AEM API GraphQL per caricare i dati, tuttavia, prima di questo, cerchiamo di comprendere gli artefatti di alto livello che è necessario distribuire.

**Creare SPA artifact dell&#39;app:**

I file prodotti dal framework SPA, in genere **HTML, CSS, JS**, noti anche come artefatti di build statici. Nel caso dell’app React, gli artefatti della `build` e per gli artefatti Vue `dist` directory.
La richiesta al tuo SPA (ad esempio https://HOST/my-aem-spa.html) verrà servita utilizzando questi artefatti di build.

**AEM API GraphQL:**

Chiaramente, questo endpoint API GraphQL (`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`) deve essere ospitata sul dominio AEM .

In sintesi, l&#39;architettura di distribuzione SPA ha due parti *1. SPA 2. Livello API AEM GraphQL* quindi esaminiamo le opzioni di distribuzione per queste due parti.


## Opzioni di distribuzione

| Opzione di distribuzione | URL SPA | URL API di AEM GraphQL | Configurazione CORS necessaria? |
| ---------|---------- | ---------|---------- |
| **Stesso dominio** | https://**HOST**/my-aem-spa.html | https://**HOST**/graphql/execute.json/.. | ✘ |
| **Dominio diverso** | https://**HOST SPA**/my-aem-spa.html | https://**HOST AEM**/graphql/execute.json/.. | ↓ |

**Stesso dominio:**\
Entrambi *Livello API SPA e AEM GraphQL* in questa opzione vengono distribuiti nel **Stesso dominio**. Richiesta di significato per SPA URI `/my-aem-spa.html` e livello API GraphQL `/graphql/execute.json/` sono serviti dallo stesso dominio esatto.

**Dominio diverso:**\
Entrambi *Livello API SPA e AEM GraphQL* in questa opzione viene distribuito in **Dominio diverso**. Richiesta di significato per SPA URI `/my-aem-spa.html` viene servito da un **dominio diverso** rispetto al livello API GraphQL `/graphql/execute.json/` richieste. Nota come parte di questa opzione di distribuzione, è necessario [configurare CORS](cors.md) sull&#39;istanza AEM.

>[!NOTE]
>
>È NECESSARIO configurare correttamente CORS AEM&#39;istanza, [vedi i passaggi qui](cors.md).

### Distribuzione sullo stesso dominio

Durante la distribuzione sullo stesso dominio, il dominio potrebbe essere un **dominio AEM principale** (Su AEM dominio) o **dominio SPA principale** (Off AEM Domain) e in entrata SPA, AEM le richieste API GraphQL possono essere suddivise in uno qualsiasi dei componenti di distribuzione come CDN ([Favoloso](https://docs.fastly.com/en/guides/routing-assets-to-different-origins), Akamai, [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)), [HTTPD con proxy inverso](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). In altre parole, si distribuiscono ancora gli artefatti SPA build e le API GraphQL AEM a server diversi, ma per gli utenti finali, questi vengono consegnati da un singolo dominio e dietro la scena indirizzata a un server di destinazione o di origine diverso.

Inoltre, SPA gli artefatti di build possono essere ospitati in AEM *ma non sono consigliati.*

| Distribuzione dello stesso dominio | Divisione CDN | HTTPD + proxy inverso | Artifact SPA ospitati AEM |
| ---------|---------- | ---------|---------- |
| **SU dominio AEM** | ↓ | ↓ | ↓ |
| **OFF AEM Domain** | ↓ | ↓ | **N/D** |


**HTTPD + proxy inverso**

Di seguito è riportato un esempio di configurazione

>[!TIP]
>
> Di seguito sono riportati alcuni esempi di configurazioni. Assicurati di regolarli per allinearli ai requisiti del progetto.

SUL DOMINIO AEM

    &quot;
    ProxyPass &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

OFF AEM Domain

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### Distribuzione su domini diversi

In questo scenario, gli artefatti di creazione SPA vengono distribuiti in un dominio diverso da quello AEM API GraphQL e per gli utenti finali, saranno quindi consegnati da due domini separati [Configurazione CORS](cors.md) è DEVE su AEM.

**Richieste SPA app tramite domini diversi**

![Distribuzione SPA dominio diversi](assets/spa/different-domain-spa-delivery.png)


**Intestazione di risposta CORS su AEM API GraphQL**

![API GraphQL AEM intestazione di risposta CORS](assets/spa/CORS-response-header-aem-graphql-api.png)


