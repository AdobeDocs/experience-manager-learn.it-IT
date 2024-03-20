---
title: Real User Monitoring (RUM)
description: Scopri di più sul Monitoraggio utenti reali (RUM) nel sito web AEM as a Cloud Service.
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# Real User Monitoring (RUM)

Scopri di più sul Monitoraggio utenti reali (RUM) nel sito web AEM as a Cloud Service. Scopri come abilitare RUM, quali dati vengono raccolti e come utilizzare i dati RUM per ottimizzare l’esperienza utente sul tuo sito web.

## Panoramica

Real User Monitoring (RUM) è un metodo utilizzato per _raccogliere, misurare e analizzare le interazioni e le esperienze degli utenti;_ con un sito web in tempo reale. Fornisce informazioni approfondite su come i visitatori del sito interagiscono con il sito web, compreso il loro comportamento, le loro prestazioni e l’esperienza complessiva. Ciò si ottiene inserendo una piccola parte di codice JavaScript nelle pagine del sito.

Utilizzando il codice JavaScript, RUM acquisisce i dati direttamente dal browser dell’utente durante l’interazione con il sito web. Questi dati possono essere utilizzati per identificare e diagnosticare problemi di prestazioni, ottimizzare l’esperienza utente e migliorare i risultati aziendali.

La funzione RUM in AEM as a Cloud Service fornisce una vista completa dell&#39;esperienza utente del sito web. Acquisisce le seguenti metriche chiave per ogni pagina (URL) visitata dall’utente:

- [LCP (Largest Contentful Paint)](https://web.dev/articles/lcp) - misura le prestazioni di carico.
- [Spostamento layout cumulativo (CLS)](https://web.dev/articles/cls) - misura la stabilità visiva.
- [Interazione con la vernice successiva (INP)](https://web.dev/articles/inp) - misura l&#39;interattività.
- Visualizzazioni pagina: misura il numero di volte in cui viene visualizzata una pagina.

Inoltre, acquisisce gli errori 404 e i grafici pageview per il sito web.

Le metriche LCP, CLS e INP fanno parte del [Elementi vitali web di base](https://web.dev/articles/vitals) che sono un insieme di metriche relative alla velocità, alla reattività e alla stabilità visiva di un sito web. Queste metriche vengono utilizzate da Google per misurare l’esperienza utente su un sito web e sono importanti per la classificazione dei motori di ricerca.

## Abilita RUM

Per abilitare RUM per il sito Web AEMCS, consulta [Come impostare il servizio Real User Monitoring](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

I dettagli chiave di RUM in AEMCS sono:

- Il RUM è applicabile solo al servizio di pubblicazione di AEMCS, il che significa che il codice JavaScript viene inserito solo nell’ambiente di pubblicazione.
- Il `com.adobe.granite.webvitals.WebVitalsConfig` La configurazione OSGi controlla i percorsi di inclusione e di esclusione: si tratta di percorsi dell’archivio e non di percorsi URL.
- Per impostazione predefinita `/content` percorso incluso.
- Per escludere i percorsi, aggiungi `AEM_WEBVITALS_EXCLUDE` Variabile di ambiente di Cloud Manager, consulta [Aggiunta di variabili di ambiente](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). I percorsi sono separati da una virgola.
- Il codice OOTB è responsabile dell’inserimento del codice JavaScript nelle pagine.

### Verifica

Per verificare se RUM è abilitato per il sito Web, visualizza l’origine HTML della pagina pubblicata e cerca i seguenti blocchi di script:

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## Raccolta dati RUM

- I dati RUM vengono raccolti utilizzando `sampleRUM()` passando il nome del punto di controllo. Nell’esempio precedente, i punti di controllo sono `top`, `load`, `click`, `lazy`, e `cwv`.
- Un punto di controllo è un evento denominato nella sequenza di caricamento della pagina e di interazione con essa.

Vedi anche [Servizio Real User Monitoring e privacy](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) e [Quali dati vengono raccolti](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) per ulteriori dettagli.

## Visualizzazione dati RUM

Per visualizzare i dati RUM necessari `domainkey`, Adobe lo fornisce come parte della configurazione di RUM. Il dashboard di RUM è disponibile all’indirizzo [https://data.aem.live/](https://data.aem.live/) e puoi accedervi utilizzando la chiave di dominio e l’url.

Ad esempio, la schermata seguente mostra la dashboard RUM per il sito web WKND dell’AEM.

![Dashboard RUM](./assets/rum/RUM-Dashboard-WKND.png)

La dashboard di RUM fornisce le seguenti informazioni chiave:

- **Metriche delle prestazioni** - LCP, CLS, INP e Pageview.
- **Metriche di errore** - 404 errori.
- **Grafici Pageview** - Numero di visualizzazioni di pagina nel tempo.

## Come utilizzare i dati RUM

Utilizzando le informazioni di cui sopra, puoi ottimizzare l’esperienza utente sul tuo sito web. Ad esempio:

- Riduci LCP, CLS e INP per migliorare le prestazioni di caricamento delle pagine e l’interattività. Consulta [Come migliorare LCP](https://web.dev/articles/lcp#improve-lcp), [Come migliorare CLS](https://web.dev/articles/cls#improve-cls) e [Come migliorare l’INP](https://web.dev/articles/inp#improve-inp)per ulteriori dettagli.
- Per migliorare l’esperienza utente, correggi gli errori 404.
- Per comprendere il comportamento degli utenti e ottimizzare il contenuto, analizzare i grafici pageview.

Adobe consiglia di rivedere regolarmente il cruscotto RUM e in particolare dopo il rilascio principale o minore.

Vedi anche [Chi può trarre vantaggio dal servizio Real User Monitoring](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) per ulteriori dettagli.
