---
title: Aggiungere il branding al sito web
description: Definisci i CSS globali, le variabili CSS e i font web per un sito Edge Delivery Services.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1313'
ht-degree: 100%

---

# Aggiungere il branding al sito web

Per iniziare, imposta il branding complessivo aggiornando gli stili globali, definendo le variabili CSS e aggiungendo i font web. Questi elementi fondamentali, che vanno applicati in modo sistematico in tutto il sito, garantiscono che il sito web sia coerente e facile da gestire.

## Creare un problema GitHub

Per mantenere tutto organizzato, utilizza GitHub per tenere traccia del lavoro. Innanzitutto, crea un problema GitHub per questo corpo del lavoro:

1. Passa all’archivio GitHub (per i dettagli, consulta il capitolo [Creare un progetto di codice](./1-new-code-project.md)).
2. Fai clic sulla scheda **Problemi** e quindi su **Nuovo problema**.
3. Scrivi un **titolo** e una **descrizione** per il lavoro da eseguire.
4. Fai clic su **Invia nuovo problema**.

Il problema GitHub viene utilizzato successivamente durante [la creazione di una richiesta pull](#merge-code-changes).

![GitHub crea un nuovo problema](./assets/4-website-branding/github-issues.png)

## Creare un ramo di lavoro

Per mantenere l’organizzazione e garantire la qualità del codice, crea un nuovo ramo per ogni corpo del lavoro. Questa procedura impedisce al nuovo codice di influire negativamente sulle prestazioni e assicura che le modifiche non siano live prima del loro completamento.

Per questo capitolo, incentrato sugli stili di base del sito web, crea un ramo denominato `wknd-styles`.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## CSS globale

Edge Delivery Services utilizza un file CSS globale, disponibile in `styles/styles.css`, per impostare gli stili comuni per l’intero sito web. Il file `styles.css` controlla aspetti quali colori, font e spaziatura, assicurandosi che in tutto il sito l’aspetto sia coerente.

I CSS globali dovrebbero essere agnostici nei confronti di costrutti di livello inferiore come i blocchi, concentrandosi sull’aspetto generale del sito e sui trattamenti visivi condivisi.

Tieni presente che gli stili CSS globali possono essere ignorati in base alle necessità.

### Variabili CSS

[Le variabili CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) sono un ottimo modo per archiviare impostazioni di progettazione come colori, font e dimensioni. Utilizzando le variabili, puoi modificare questi elementi in un’unica posizione e averli aggiornati in tutto il sito.

Per iniziare a personalizzare le variabili CSS, segui questi passaggi:

1. Apri il file `styles/styles.css` nell’editor di codice.
2. Trova la dichiarazione `:root`, in cui sono memorizzate le variabili CSS globali.
3. Modifica le variabili di colore e font in modo che corrispondano al brand WKND.

Esempio:


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

Esplora le altre variabili nella sezione `:root` e controlla le impostazioni predefinite.

Quando sviluppi un sito web ed è necessario ripetere gli stessi valori CSS, puoi creare nuove variabili per semplificare la gestione degli stili. Esempi di altre proprietà CSS che possono usufruire delle variabili CSS, includono: `border-radius`, `padding`, `margin` e `box-shadow`.

### Elementi nudi

Gli elementi nudi hanno lo stile applicato direttamente tramite il nome dell’elemento invece di utilizzare una classe CSS. Ad esempio, anziché applicare lo stile a una classe CSS `.page-heading`, gli stili vengono applicati all’elemento `h1` utilizzando `h1 { ... }`.

Nel file `styles/styles.css`, un set di stili di base viene applicato agli elementi HTML nudi. I siti web di Edge Delivery Services danno la priorità all’utilizzo di elementi nudi perché si allineano con HTML, semantica nativa di Edge Delivery Services.

Per allinearsi al branding WKND, applica uno stile ad alcuni elementi nudi in `styles.css`:

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

Questi stili garantiscono che `h2` elementi, a meno che non vengano sostituiti, abbiano uno stile coerente con il branding WKND, contribuendo a creare una chiara gerarchia visiva. La sottolineatura gialla parziale sotto ogni `h2` aggiunge un tocco distintivo alle intestazioni.

### Elementi dedotti

In Edge Delivery Services, il codice `scripts.js` e `aem.js` del progetto ottimizzano automaticamente elementi HTML semplici specifici in base al relativo contesto all’interno dell’HTML.

Ad esempio, gli elementi di ancoraggio (`<a>`) creati su una propria riga, anziché in linea con il testo circostante, sono dedotti come pulsanti in base a questo contesto. Questi ancoraggi vengono racchiusi automaticamente in un contenitore `div` con classe CSS `button-container` e all’elemento di ancoraggio viene aggiunta una classe CSS `button`.

Ad esempio, quando un collegamento viene creato su una propria riga, Edge Delivery Services JavaScript aggiorna il DOM come segue:

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

Questi pulsanti possono essere personalizzati in modo da corrispondere al brand WKND, che stabilisce che i pulsanti vengano visualizzati come rettangoli gialli con testo nero.

Di seguito è riportato un esempio di come assegnare uno stile ai “pulsanti dedotti”&quot; in `styles.css`:

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

Questo CSS definisce gli stili di base dei pulsanti e include trattamenti specifici per WKND, ad esempio il testo in maiuscolo, lo sfondo giallo e il testo in nero. Le proprietà `background-color` e `color` utilizzano variabili CSS che consentono allo stile del pulsante di rimanere allineato ai colori del brand. Questo approccio garantisce che i pulsanti abbiano uno stile coerente all’interno del sito, mantenendo al contempo la flessibilità.

## Font web

I progetti Edge Delivery Services ottimizzano l’utilizzo dei font web al fine di mantenere prestazioni elevate e ridurre al minimo l’impatto sui punteggi di Lighthouse. Questo metodo assicura un rendering rapido senza compromettere l’identità visiva del sito. Ecco come implementare i font web in modo efficiente per ottenere prestazioni ottimali.

### Tipi di font

Aggiungi font web personalizzati utilizzando le dichiarazioni CSS `@font-face` nel file `styles/fonts.css`. L’aggiunta di `@font-faces` a `fonts.css` garantisce il caricamento dei font web nel momento ottimale, contribuendo a mantenere i punteggi di Lighthouse.

1. Apri `styles/fonts.css`.
2. Aggiungi le seguenti dichiarazioni `@font-face` per includere i font del brand WKND: `Asar` e `Source Sans Pro`.

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

I font utilizzati in questo tutorial provengono da Google Fonts, ma i font web possono essere ottenuti da qualsiasi provider di font, incluso [Adobe Fonts](https://fonts.adobe.com/).

+++Utilizzo di file dei font web locali

In alternativa, i file dei font web vengono copiati nel progetto nella cartella `/fonts` e a cui si fa riferimento nelle dichiarazioni `@font-face`.

Questo tutorial utilizza i font web remoti, in hosting, così da poterlo seguire più facilmente.

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

Infine, aggiorna le variabili CSS `styles/styles.css` per utilizzare i nuovi font:

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback` e `roboto-condensed-fallback` sono font di fallback che vengono aggiornati nella sezione [Font di fallback](#fallback-fonts) per allinearsi e supportare i font web `Asar` e `Source Sans Pro` personalizzati.

### Font di fallback

I font web spesso influiscono sulle prestazioni a causa delle relative dimensioni, aumentando potenzialmente i punteggi di CLS (Cumulative Layout Shift) e riducendo i punteggi complessivi di Lighthouse. Per garantire la visualizzazione immediata del testo durante il caricamento dei font web, nei progetti Edge Delivery Services vengono utilizzati caratteri di fallback nativi per il browser. Questo approccio consente di mantenere un’esperienza utente fluida pur applicando il font desiderato.

Per selezionare il font di fallback migliore, utilizza l’estensione per [Chrome Helix Font Fallback](https://www.aem.live/developer/font-fallback) di Adobe, che individua un font con maggiore corrispondenza da utilizzare nei browser prima del caricamento del font personalizzato. Le dichiarazioni dei font di fallback risultanti devono essere aggiunte al file `styles/styles.css` per migliorare le prestazioni e garantire un’esperienza fluida per gli utenti.

![Estensione Helix Font Fallback Chrome](./assets/4-website-branding/font-fallback-chrome-plugin.png){align=center}

Per utilizzare l’estensione [Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback), assicurati che alla pagina web siano applicati i font web nelle stesse varianti utilizzate nel sito web di Edge Delivery Services. In questo tutorial viene illustrata l’estensione in [wknd.site](http://wknd.site/us/en.html). Durante lo sviluppo di un sito web, applica l’estensione al sito su cui stai lavorando anziché a [wknd.site](http://wknd.site/us/en.html).

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

Aggiungi i nomi delle famiglie di font di fallback alle variabili dei font CSS in `styles/styles.css` dopo i nomi delle famiglie di font “reali”.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## Anteprima di sviluppo

Man mano che aggiungi CSS, l’ambiente di sviluppo locale CLI di AEM ricarica automaticamente le modifiche, rendendo più facile e veloce visualizzare in che modo tale aggiunta influisce sul blocco.

![Anteprima di sviluppo CSS del brand WKND](./assets/4-website-branding/preview.png)


## Scaricare i file CSS finali

Puoi scaricare i file CSS aggiornati dai collegamenti seguenti:

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## Eseguire il linting dei file CSS

Assicurati di [eseguire frequentemente il linting](./3-local-development-environment.md#linting) delle modifiche al codice per garantire che sia pulito e coerente. Eseguire il linting, consente di rilevare i problemi in anticipo e riduce il tempo di sviluppo complessivo. Ricorda che non è possibile unire il tuo lavoro al ramo principale finché non saranno stati risolti tutti i problemi di linting.

```bash
$ npm run lint:css
```

## Unire le modifiche al codice

Unisci le modifiche al ramo `main` su GitHub per creare il lavoro futuro su questi aggiornamenti.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

Una volta trasmesse le modifiche nel ramo `wknd-styles`, crea una richiesta pull su GitHub per unirle al ramo `main`.

1. Passa all’archivio GitHub dal capitolo [Crea un nuovo progetto](./1-new-code-project.md).
2. Fai clic sulla scheda **Richieste pull** e seleziona **Nuova richiesta pull**.
3. Imposta `wknd-styles` come ramo di origine e `main` come ramo di destinazione.
4. Rivedi le modifiche e fai clic su **Crea richiesta pull**.
5. Nei dettagli della richiesta pull, **aggiungi quanto segue**:

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1` fa riferimento al problema GitHub creato in precedenza.
   * Gli URL di test indicano ad AEM Code Sync quali rami utilizzare per la convalida e il confronto. L’URL “After” utilizza il ramo di lavoro `wknd-styles` per verificare in che modo le modifiche al codice influiscono sulle prestazioni del sito web.

6. Fai clic su **Crea richiesta pull**.
7. Attendi che l’app ](./1-new-code-project.md)AEM Code Sync di GitHub[ **completi i controlli di qualità**. In caso contrario, risolvi gli errori ed esegui nuovamente i controlli.
8. Una vola superati i controlli, **unisci la richiesta pull** in `main`.

Le modifiche unite in `main` non vengono considerate distribuite in produzione e il nuovo sviluppo può procedere in base a questi aggiornamenti.
