---
title: Aggiungere il marchio del sito Web
description: Definisci le variabili CSS, CSS e i font Web globali per un sito Edge Delivery Services.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---


# Aggiungere il marchio del sito Web

Per iniziare, imposta il marchio complessivo aggiornando gli stili globali, definendo le variabili CSS e aggiungendo i font web. Questi elementi fondamentali garantiscono che il sito web rimanga coerente e gestibile e che sia applicato in modo coerente in tutto il sito.

## Crea un problema GitHub

Per mantenere tutto organizzato, utilizza GitHub per monitorare il lavoro. Innanzitutto, crea un problema GitHub per questo corpo di lavoro:

1. Vai all&#39;archivio GitHub (per i dettagli, consulta il capitolo [Creare un progetto di codice](./1-new-code-project.md)).
2. Fai clic sulla scheda **Issues** e quindi su **New issue**.
3. Scrivi un **titolo** e una **descrizione** per il lavoro da eseguire.
4. Fai clic su **Invia nuovo problema**.

Il problema GitHub viene utilizzato successivamente quando [si crea una richiesta di pull](#merge-code-changes).

![GitHub crea un nuovo problema](./assets/4-website-branding/github-issues.png)

## Creare un ramo di lavoro

Per mantenere l’organizzazione e garantire la qualità del codice, crea un nuovo ramo per ogni corpo di lavoro. Questa procedura impedisce al nuovo codice di influire negativamente sulle prestazioni e assicura che le modifiche non siano attive prima del loro completamento.

Per questo capitolo, che si concentra sugli stili di base del sito Web, creare un ramo denominato `wknd-styles`.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## CSS globale

Edge Delivery Services utilizza un file CSS globale, che si trova in `styles/styles.css`, per impostare gli stili comuni per l&#39;intero sito Web. `styles.css` controlla aspetti quali colori, font e spaziatura, assicurandosi che tutto sembri coerente nel sito.

I CSS globali dovrebbero essere agnostici nei confronti di costrutti di livello inferiore come i blocchi, concentrandosi sull’aspetto generale del sito e sui trattamenti visivi condivisi.

Tieni presente che gli stili CSS globali possono essere ignorati quando necessario.

### Variabili CSS

[Le variabili CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) sono un ottimo modo per memorizzare impostazioni di progettazione come colori, font e dimensioni. Utilizzando le variabili, puoi modificare questi elementi in un’unica posizione e aggiornarli in tutto il sito.

Per iniziare a personalizzare le variabili CSS, segui questi passaggi:

1. Aprire il file `styles/styles.css` nell&#39;editor di codice.
2. Trovare la dichiarazione `:root`, in cui sono memorizzate le variabili CSS globali.
3. Modifica le variabili di colore e font in modo che corrispondano al marchio WKND.

Ecco un esempio:


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

Quando sviluppi un sito web e ti ritrovi a ripetere gli stessi valori CSS, puoi creare nuove variabili per semplificare la gestione degli stili. Esempi di altre proprietà CSS che possono beneficiare delle variabili CSS, includono: `border-radius`, `padding`, `margin` e `box-shadow`.

### Elementi bare

Gli elementi nudi sono formattati direttamente tramite il nome del loro elemento invece di utilizzare una classe CSS. Ad esempio, anziché applicare lo stile a una classe CSS `.page-heading`, gli stili vengono applicati all&#39;elemento `h1` utilizzando `h1 { ... }`.

Nel file `styles/styles.css`, un set di stili di base viene applicato agli elementi bare HTML. I siti web di Edge Delivery Services assegnano la priorità utilizzando gli elementi nudi perché sono allineati con il HTML semantico nativo del servizio Edge Delivery.

Per allinearsi al branding WKND, diamo uno stile ad alcuni elementi nudi in `styles.css`:

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

In Edge Delivery Services, il codice `scripts.js` e `aem.js` del progetto ottimizza automaticamente elementi bare HTML specifici in base al loro contesto all&#39;interno del HTML.

Ad esempio, gli elementi di ancoraggio (`<a>`) creati sulla propria riga, anziché in linea con il testo circostante, sono considerati pulsanti basati su questo contesto. Questi ancoraggi vengono racchiusi automaticamente in un contenitore `div` con classe CSS `button-container` e all&#39;elemento di ancoraggio viene aggiunta una classe CSS `button`.

Ad esempio, quando un collegamento viene creato su una propria riga, Edge Delivery Services JavaScript aggiorna il DOM come segue:

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

Questi pulsanti possono essere personalizzati in modo da corrispondere al marchio WKND, che determina la visualizzazione dei pulsanti come rettangoli gialli con testo nero.

Di seguito è riportato un esempio di come assegnare uno stile ai &quot;pulsanti dedotti&quot; in `styles.css`:

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

Questo CSS definisce gli stili dei pulsanti di base e include trattamenti specifici per WKND, ad esempio testo in maiuscolo, sfondo giallo e testo nero. Le proprietà `background-color` e `color` utilizzano variabili CSS che consentono allo stile del pulsante di rimanere allineato con i colori del brand. Questo approccio garantisce che i pulsanti abbiano uno stile coerente all&#39;interno del sito e rimangano flessibili.

## Font web

I progetti di Edge Delivery Services ottimizzano l’uso dei font per web al fine di mantenere prestazioni elevate e ridurre al minimo l’impatto sui punteggi di Lighthouse. Questo metodo assicura un rendering rapido senza compromettere l’identità visiva del sito. Ecco come implementare i font web in modo efficiente per ottenere prestazioni ottimali.

### Facce font

Aggiungere font Web personalizzati utilizzando le dichiarazioni CSS `@font-face` nel file `styles/fonts.css`. L&#39;aggiunta di `@font-faces` a `fonts.css` garantisce il caricamento ottimale dei caratteri Web, contribuendo a mantenere i punteggi di Lighthouse.

1. Apri `styles/fonts.css`.
2. Aggiungi le seguenti `@font-face` dichiarazioni per includere i font del brand WKND: `Asar` e `Source Sans Pro`.

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

I tipi di carattere utilizzati in questa esercitazione provengono da Google Fonts, ma i tipi di carattere Web possono essere ottenuti da qualsiasi provider di tipi di carattere, incluso [Adobe Fonts](https://fonts.adobe.com/).

+++Utilizzo di file di font Web locali

In alternativa, i file di font Web vengono copiati nel progetto nella cartella `/fonts` e a cui si fa riferimento nelle dichiarazioni `@font-face`.

Questo tutorial utilizza i font web remoti, in hosting, per facilitarne il follow-up.

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

`roboto-fallback` e `roboto-condensed-fallback` sono tipi di carattere di fallback aggiornati nella sezione [Tipi di carattere di fallback](#fallback-fonts) per l&#39;allineamento al supporto dei tipi di carattere Web `Asar` e `Source Sans Pro` personalizzati.

### Font di fallback

I font web spesso influiscono sulle prestazioni a causa delle loro dimensioni, aumentando potenzialmente i punteggi CLS (Cumulative Layout Shift) e riducendo i punteggi complessivi di Lighthouse. Per garantire la visualizzazione immediata del testo durante il caricamento dei font Web, i progetti di Edge Delivery Services utilizzano font di fallback nativi per il browser. Questo approccio consente di mantenere un’esperienza utente fluida mentre viene applicato il font desiderato.

Per selezionare il tipo di carattere di fallback ottimale, utilizzare l&#39;estensione di Adobe [Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback), che determina un tipo di carattere strettamente corrispondente per i browser da utilizzare prima del caricamento del tipo di carattere personalizzato. Le dichiarazioni font di fallback risultanti devono essere aggiunte al file `styles/styles.css` per migliorare le prestazioni e garantire un&#39;esperienza fluida per gli utenti.

Per utilizzare l&#39;estensione [Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback), verificare che alla pagina Web siano applicati i tipi di carattere Web nelle stesse varianti utilizzate nel sito Web dei Edge Delivery Services. Questa esercitazione illustra l&#39;estensione in [wknd.site](http://wknd.site/us/en.html). Durante lo sviluppo di un sito Web, applicare l&#39;estensione al sito su cui si sta lavorando anziché a [wknd.site](http://wknd.site/us/en.html).

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

Aggiungere i nomi delle famiglie di font di fallback alle variabili CSS font in `styles/styles.css` dopo i nomi delle famiglie di font &quot;reali&quot;.

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

Quando si aggiunge un file CSS, l’ambiente di sviluppo locale della CLI dell’AEM ricarica automaticamente le modifiche, rendendo più rapido e semplice vedere in che modo il file CSS influisce sul blocco.

![Anteprima di sviluppo del marchio WKND CSS](./assets/4-website-branding/preview.png)


## Scarica i file CSS finali

Puoi scaricare i file CSS aggiornati dai collegamenti seguenti:

* [`styles.css`](https://adobe.com#TODO)
* [`fonts.css`](https://adobe.com#TODO)

## Collega i file CSS

Assicurati di [collegare frequentemente](./3-local-development-environment.md#linting) le modifiche al codice per assicurarti che siano pulite e coerenti. La colorazione consente di risolvere i problemi in anticipo e riduce i tempi di sviluppo complessivi. Non è possibile unire il lavoro al ramo principale finché non vengono risolti tutti i problemi di linting.

```bash
$ npm run lint:css
```

## Unisci modifiche codice

Unisci le modifiche nel ramo `main` su GitHub per generare il lavoro futuro su questi aggiornamenti.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

Dopo il push delle modifiche al ramo `wknd-styles`, crea una richiesta pull su GitHub per unirle al ramo `main`.

1. Passa all&#39;archivio GitHub dal capitolo [Crea un nuovo progetto](./1-new-code-project.md).
2. Fai clic sulla scheda **Richieste pull** e seleziona **Nuova richiesta pull**.
3. Imposta `wknd-styles` come ramo di origine e `main` come ramo di destinazione.
4. Rivedi le modifiche e fai clic su **Crea richiesta di pull**.
5. Nei dettagli della richiesta di pull, **aggiungi quanto segue**:

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1` fa riferimento al problema GitHub creato in precedenza.
   * Gli URL di test indicano a AEM Code Sync quali rami utilizzare per la convalida e il confronto. L&#39;URL &quot;After&quot; (Dopo) utilizza il ramo di lavoro `wknd-styles` per verificare in che modo le modifiche al codice influiscono sulle prestazioni del sito Web.

6. Fai clic su **Crea richiesta di pull**.
7. Attendi che l&#39;app GitHub ](./1-new-code-project.md) per la sincronizzazione del codice AEM [completi i controlli di qualità&#x200B;**.** In caso contrario, risolvere gli errori ed eseguire nuovamente i controlli.
8. Al termine dei controlli, **unisci la richiesta di pull** in `main`.

Le modifiche unite in `main` non vengono considerate distribuite in produzione e il nuovo sviluppo può procedere in base a questi aggiornamenti.
