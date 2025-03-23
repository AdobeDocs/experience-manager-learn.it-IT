---
title: Opzioni del nome di dominio personalizzato
description: Scopri come gestire e implementare nomi di dominio personalizzati per il sito web ospitato da AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
exl-id: e11ff38c-e823-4631-a5b0-976c2d11353e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 1%

---

# Opzioni del nome di dominio personalizzato

Scopri come gestire e implementare i nomi di dominio per il sito web ospitato da AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## Prima di iniziare

Prima di iniziare a implementare i nomi di dominio personalizzati, assicurati di comprendere i seguenti concetti:

### Che cos’è un nome di dominio

Un nome di dominio è il nome descrittivo del sito Web, ad esempio adobe.com, che punta a una posizione specifica (indirizzo IP come 170.2.14.16) su Internet.

### Nomi di dominio predefiniti in AEM as a Cloud Service

Per impostazione predefinita, ad AEM as a Cloud Service viene fornito un nome di dominio predefinito, che termina con `*.adobeaemcloud.com`. Il certificato SSL con caratteri jolly emesso nei confronti di `*.adobeaemcloud.com` viene applicato automaticamente a tutti gli ambienti e questo certificato con caratteri jolly è di responsabilità di Adobe.

I nomi di dominio predefiniti sono nel formato `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com`.

- `<SERVICE-TYPE>` potrebbe essere **author**, **publish** o **preview**.
- `<PROGRAM-ID>` è l&#39;identificatore univoco del programma. Un’organizzazione può avere più programmi.
- `<ENVIRONMENT-ID>` è l&#39;identificatore univoco dell&#39;ambiente e ogni programma contiene quattro ambienti: **Sviluppo rapido (RDE)**, **dev**, **stage** e **prod**. Ogni ambiente contiene i tre tipi di servizio sopra indicati, ad eccezione di **RDE** che non dispone di un ambiente di anteprima.

In sintesi, una volta eseguito il provisioning di tutti gli ambienti AEM as a Cloud Service, si dispone di **11** (RDE non dispone di un ambiente di anteprima) URL univoci combinati con il nome di dominio predefinito.

### CDN gestita da Adobe e CDN gestita dal cliente

Per ridurre la latenza e migliorare le prestazioni del sito web, AEM as a Cloud Service è integrato con una rete CDN (Content Delivery Network) gestita da Adobe. La rete CDN gestita da Adobe viene abilitata automaticamente per tutti gli ambienti. Per ulteriori dettagli, vedi [Memorizzazione in cache di AEM as a Cloud Service](../caching/overview.md).

Tuttavia, i clienti possono anche utilizzare la propria rete CDN, denominata **rete CDN gestita dal cliente**. Non è necessario, ma pochi clienti lo utilizzano per politiche aziendali o altri motivi. In questo caso, il cliente è responsabile della gestione delle configurazioni e delle impostazioni CDN.

### Nomi di dominio personalizzati

I nomi di dominio personalizzati sono sempre preferiti ai nomi di dominio predefiniti a scopo di branding, autenticità e sviluppo aziendale. Tuttavia, possono essere applicate solo ai tipi di servizio **publish** e **preview** e non a **author**.

Quando aggiungi nomi di dominio personalizzati, devi fornire un certificato SSL valido per il dominio personalizzato specificato. Il certificato SSL deve essere un certificato valido firmato da un’autorità di certificazione (CA) attendibile.

In genere, i clienti utilizzano un nome di dominio personalizzato per gli ambienti di produzione (sito Web AEM as a Cloud Service) e talvolta per ambienti inferiori come **stage** o **dev**.

| Tipo di servizio AEM | Dominio personalizzato supportato? |
|---------------------|:-----------------------:|
| Autore | ✘ |
| Anteprima | ✔ |
| Pubblicazione | ✔ |

## Implementazione dei nomi di dominio

Per implementare i nomi di dominio utilizzando una rete CDN gestita da Adobe o una rete CDN gestita dal cliente, il seguente diagramma di flusso ti guida attraverso la procedura:

![Diagramma di flusso della gestione dei nomi di dominio](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

Inoltre, la tabella seguente ti guida dove gestire le configurazioni specifiche:

| Nome di dominio personalizzato con | Aggiungi certificato SSL a | Aggiungi nome dominio a | Configurare i record DNS in | Hai bisogno della regola CDN di convalida dell’intestazione HTTP? |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| CDN gestita da Adobe | Adobe Cloud Manager | Adobe Cloud Manager | Servizio di hosting DNS | ✘ |
| CDN gestita dal cliente | Fornitore CDN | Fornitore CDN | Servizio di hosting DNS | ✔ |

### Tutorial dettagliati

Ora che conosci il processo di gestione dei nomi di dominio, puoi implementare nomi di dominio personalizzati per il tuo sito web AEM as a Cloud Service seguendo le esercitazioni seguenti:

**[Nomi di dominio personalizzati con rete CDN gestita da Adobe](./custom-domain-name-with-adobe-managed-cdn.md)**: in questa esercitazione imparerai ad aggiungere un nome di dominio personalizzato a un sito Web di **AEM as a Cloud Service con rete CDN gestita da Adobe**.
**[Nomi di dominio personalizzati con CDN gestita dal cliente](./custom-domain-names-with-customer-managed-cdn.md)**: in questa esercitazione imparerai ad aggiungere un nome di dominio personalizzato a un **sito Web AEM as a Cloud Service con CDN gestita dal cliente**.
