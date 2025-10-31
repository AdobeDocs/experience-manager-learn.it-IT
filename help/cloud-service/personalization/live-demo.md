---
title: Demo live dei casi di utilizzo di Personalization
description: Personalizzazione delle esperienze in azione sul sito web WKND Enablement con test A/B, targeting comportamentale ed esempi di personalizzazione per utenti noti.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations
role: Developer, Architect, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-11-01T00:00:00Z
jira: KT-19546
thumbnail: KT-19546.jpeg
source-git-commit: 055dc7d666d082244d73d3494bac54d7eb4bb886
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 1%

---

# Demo live dei casi di utilizzo di Personalization

Visita il [sito Web WKND Enablement](https://wknd.enablementadobe.com/us/en.html){target="wknd"} per visualizzare esempi reali di test A/B, targeting comportamentale e personalizzazione di utenti noti.

Questa pagina ti guida attraverso dimostrazioni pratiche di ogni scenario di personalizzazione. Utilizzalo per esplorare le possibilità prima di creare queste funzionalità sul tuo sito AEM.

>[!IMPORTANT]
>
> Apri il sito demo in più finestre del browser o in modalità di navigazione in incognito/privata per sperimentare diverse varianti personalizzate contemporaneamente.
> &#x200B;> Quando si utilizza la modalità di navigazione privata, Firefox e Safari potrebbero bloccare il cookie ECID, in alternativa utilizzare la modalità di navigazione regolare o cancellare i cookie prima di provare un nuovo scenario di personalizzazione.

## Casi d’uso demo

Il sito Web di abilitazione [WKND](https://wknd.enablementadobe.com/us/en.html){target="wknd"} illustra tre tipi di personalizzazione:

| Tipo Personalization | Cosa vedrai | Tempistica |
|---------------------|-----------------|---------|
| **Targeting comportamentale** | I contenuti si adattano in base al comportamento e agli interessi di navigazione. Di solito indicata come _personalizzazione della pagina successiva o della stessa pagina_ | Real-time e batch |
| **Personalization utente noto** | Esperienze personalizzate basate su profili cliente completi, creati da dati su più sistemi. Di solito indicata come _personalizzazione su scala_ | Tempo reale |
| **Test A/B** | Sono state testate diverse varianti di contenuto per individuare la migliore prestazione. Solitamente denominato _sperimentazione_ | Tempo reale |

## Targeting comportamentale

Il contenuto si adatta automaticamente in base alle azioni e agli interessi del visitatore durante la sessione di navigazione. Questa operazione viene comunemente definita _personalizzazione della pagina successiva o della stessa pagina_.

### Pagine Home, Avventure e Magazine

Queste esperienze vengono visualizzate immediatamente in base al comportamento di navigazione corrente (personalizzazione in tempo reale). L’Edge Network di Adobe Experience Platform viene utilizzato per prendere decisioni di personalizzazione in tempo reale.

| Pagina | Cosa vedrai | Come eseguire il test | Esperienza |
|------|-----------------|-------------|------------|
| [Home](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Un banner eroe personalizzato **Adventures per famiglie** con una famiglia che si sposta in bici accanto al lago, promuovendo esperienze guidate che creano ricordi condivisi | Visita il [Bali Surf Camp](https://wknd.enablementadobe.com/us/en/adventures/bali-surf-camp.html){target="wknd"} o il [Gastronomic Marais Tour](https://wknd.enablementadobe.com/us/en/adventures/gastronomic-marais-tour.html){target="wknd"}, quindi torna alla home page | ![Home - Eroe delle avventure familiari](./assets/live-demo/behavioral-home-family-hero.png){width="200" zoomable="yes"} |
| [Avventure](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un eroe promozionale **incentrato sul ciclismo**&quot;Free Bike Tune Up&quot; con messaggi &quot;We&#39;ve Got You Covered&quot; e offerte di manutenzione bici gratuite dai partner esperti di WKND | Visita tutte le avventure correlate alla bicicletta (ad esempio, [Ciclismo in Toscana](https://wknd.enablementadobe.com/us/en/adventures/cycling-tuscany.html){target="wknd"}), quindi accedi alla pagina Avventure | ![Avventure - Ottimizzazione bici gratuita per eroe](./assets/live-demo/behavioral-adventures-bike-hero.png){width="200" zoomable="yes"} |
| [Avventure](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un **eroe della collezione di attrezzi** a tema campeggio che mostra attrezzature da campeggio essenziali (sacchi a pelo, giacche, stivali) con messaggi &quot;La tua prossima avventura inizia con l&#39;attrezzo giusto&quot; | Visita qualsiasi avventura relativa al campeggio (ad esempio, [Yosemite Backpacking](https://wknd.enablementadobe.com/us/en/adventures/yosemite-backpacking.html){target="wknd"}), quindi accedi alla pagina Avventure | ![Avventure - Eroe raccolta attrezzi Camp](./assets/live-demo/behavioral-adventures-camp-hero.png){width="200" zoomable="yes"} |
| [Rivista](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Una promozione di vendita di **riviste** con riviste WKND con &quot;SALE!&quot; di rilievo distintivi e prezzi speciali per i lettori su problemi e raccolte all’aperto | Leggi uno o più articoli della rivista (ad esempio, [Ski Touring](https://wknd.enablementadobe.com/us/en/magazine/ski-touring.html){target="wknd"}), quindi passa alla pagina di destinazione della rivista | ![Rivista - Vendite eroiche](./assets/live-demo/behavioral-magazine-sale-hero.png){width="200" zoomable="yes"} |

### Pagine delle avventure e delle riviste (batch)

Queste esperienze si basano su comportamenti storici e vengono visualizzate nella visita successiva o più tardi nello stesso giorno (personalizzazione batch). I dati vengono aggregati ed elaborati in attributi di profilo e attivati in Adobe Experience Platform Edge Network.

| Pagina | Cosa vedrai | Come eseguire il test | Esperienza |
|------|-----------------|-------------|------------|
| [Avventure](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un eroe a tema surf con **tavole da surf colorate sotto le palme** con messaggi &quot;Il tuo Percorso di surf inizia qui&quot; e contenuti curati di destinazione di surf in base ai tuoi interessi | Visita più [avventure correlate al surf](https://wknd.enablementadobe.com/us/en/adventures.html#tabs-b4210c6ff3-item-b411b19941-tab){target="wknd"}, quindi torna alla pagina Avventure il giorno successivo | ![Avventure - Surf Hero](./assets/live-demo/behavioral-adventures-surfing-hero.png){width="200" zoomable="yes"} |
| [Rivista](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Un&#39;offerta di abbonamento a **riviste personalizzate** con destinazioni di viaggio in tutto il mondo e un classico furgone VW, che enfatizza &quot;La tua esperienza di rivista personalizzata&quot; con vantaggi esclusivi per gli abbonati | Leggi 3 o più [articoli della rivista](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"}, quindi torna alla pagina di destinazione della rivista il giorno successivo | ![Rivista - Sottoscrivi eroe](./assets/live-demo/behavioral-magazine-subscribe-hero.png){width="200" zoomable="yes"} |

**Ulteriori informazioni:** Sei pronto a implementare il targeting comportamentale sul tuo sito AEM? Inizia con l&#39;[esercitazione sul targeting comportamentale](./use-cases/behavioral-targeting.md) per scoprire il processo di installazione completo.

## Personalization per utenti noti

Esperienze personalizzate basate su profili cliente completi, creati da dati su più sistemi, tra cui cronologia degli acquisti e fase del ciclo di vita del cliente. L’Edge Network di Adobe Experience Platform viene utilizzato per prendere decisioni di personalizzazione in tempo reale.

### Home page protagonista

Il banner principale della home page WKND è personalizzato in base ai profili utente autenticati. Testa con questi account demo per visualizzare un’esperienza personalizzata:

| Pagina | Cosa vedrai | Come eseguire il test | Contesto profilo | Esperienza |
|------|-----------------|-------------|-----------------|------------|
| [Home](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Un negozio di sci interno che mostra l&#39;attrezzatura da sci **premium con &quot;EXTRA 25% OFF&quot;** promozione, con consigli di imballaggio esperti per prepararsi alla loro prossima avventura di sci | Accedi con `teddy/teddy` o `asmith/asmith` e aggiorna la pagina | Avventure di sci acquistate di recente, per vendere in upselling attrezzatura da sci | ![Home - Vendita attrezzatura da sci](./assets/live-demo/known-user-ski-gear-hero.png){width="200" zoomable="yes"} |

**Ulteriori informazioni:** Sei pronto a implementare la personalizzazione degli utenti noti sul tuo sito AEM? Inizia con [Esercitazione su Personalization per utenti noti](./use-cases/known-user-personalization.md) per scoprire il processo di installazione completo.

## Test A/B (sperimentazione)

Sottoponi a test diverse varianti di contenuto per determinare quali prestazioni soddisfano al meglio gli obiettivi aziendali. Adobe Target fornisce in modo casuale diverse varianti ai visitatori e traccia con prestazioni migliori. Questa operazione viene comunemente definita _sperimentazione_.

### Articolo in primo piano sulla home page

La home page di WKND esegue un test A/B attivo con tre varianti dell&#39;_Campeggio in Western Australia_ articolo in primo piano. Ogni visitatore viene assegnato in modo casuale per visualizzare una delle seguenti varianti:

| Pagina | Cosa vedrai | Come eseguire il test | Esperienza |
|------|-----------------|-------------|------------|
| [Home](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Una delle tre varianti di articolo in primo piano assegnate casualmente nella sezione &quot;In primo piano&quot;: **&quot;Off the Grid: Epic Camping Routes Across Western Australia&quot;** o **&quot;Wandering the Wild: Camping Adventures in Western Australia&quot;** (o una terza variante), ognuna con immagini e messaggi univoci per testare quale risuona meglio | Visita la home page in diversi browser, utilizza la modalità in incognito/privata o cancella i cookie per visualizzare diverse varianti | ![Varianti Di Test A/B](./assets/live-demo/ab-test-variations.png){width="200" zoomable="yes"} |

**Ulteriori informazioni:** Sei pronto a implementare test A/B sul tuo sito AEM? Inizia con l&#39;esercitazione [Sperimentazione (test A/B)](./use-cases/experimentation.md) per scoprire il processo di installazione completo.


## Passaggi successivi

Sei pronto a implementare la personalizzazione sul tuo sito AEM? Inizia con [Panoramica di Personalization](./overview.md) per scoprire il processo di installazione completo.


