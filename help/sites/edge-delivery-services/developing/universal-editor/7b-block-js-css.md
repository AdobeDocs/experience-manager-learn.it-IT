---
title: Sviluppare un blocco con CSS e JS
description: Sviluppa un blocco con CSS e JavaScript per Edge Delivery Services, modificabile tramite l’Editor universale.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 41c4cfcf-0813-46b7-bca0-7c13de31a20e
source-git-commit: ecd3ce33204fa6f3f2c27ebf36e20ec26e429981
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---

# Sviluppare un blocco con CSS e JavaScript

Nel [capitolo precedente](./7b-block-js-css.md), era incluso lo stile di un blocco che utilizzava solo CSS. Ora l’attenzione si sposta sullo sviluppo di un blocco con JavaScript e CSS.

Questo esempio mostra come migliorare un blocco in tre modi:

1. Aggiunta di classi CSS personalizzate.
1. Utilizzo dei listener di eventi per aggiungere movimento.
1. Gestione di termini e condizioni che possono essere facoltativamente inclusi nel testo del teaser.

## Casi d’uso comuni

Questo approccio è particolarmente utile nei seguenti scenari:

- **Gestione CSS esterna:** quando il CSS del blocco viene gestito all&#39;esterno dei Edge Delivery Services e non è allineato alla struttura HTML.
- **Attributi aggiuntivi:** Quando sono necessari attributi aggiuntivi, ad esempio [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) per l&#39;accessibilità o [microdati](https://developer.mozilla.org/en-US/docs/Web/HTML/Microdata).
- **Miglioramenti di JavaScript:** Quando sono necessarie funzionalità interattive, come i listener di eventi.

Questo metodo si basa sulla manipolazione DOM di JavaScript nativa del browser, ma richiede cautela quando si modifica il DOM, in particolare quando si spostano gli elementi. Tali modifiche possono interrompere l&#39;esperienza di authoring dell&#39;editor universale. Idealmente, il [modello di contenuto](./5-new-block.md#block-model) del blocco dovrebbe essere progettato in modo accurato per ridurre al minimo la necessità di modifiche DOM estese.

## Blocca HTML

Per avvicinarti allo sviluppo dei blocchi, inizia esaminando il DOM esposto dai Edge Delivery Services. La struttura viene migliorata con JavaScript e formattata con CSS.

>[!BEGINTABS]

>[!TAB DOM da decorare]

Di seguito è riportato il DOM del blocco teaser che è la destinazione da decorare utilizzando JavaScript e CSS.

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB Come trovare il DOM]

Per trovare il DOM da decorare, apri la pagina con il blocco non decorato nel tuo ambiente di sviluppo locale, seleziona il blocco e controlla il DOM.

![DOM a blocchi Inspect](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## Blocca JavaScript

Per aggiungere funzionalità JavaScript a un blocco, creare un file JavaScript nella directory del blocco con lo stesso nome del blocco, ad esempio `/blocks/teaser/teaser.js`.

Il file JavaScript deve esportare una funzione predefinita:

```javascript
export default function decorate(block) { ... }
```

La funzione predefinita utilizza l’elemento/albero DOM che rappresenta il blocco in Edge Delivery Services HTML e contiene il JavaScript personalizzato eseguito al momento del rendering del blocco.

In questo esempio JavaScript esegue tre azioni principali:

1. Aggiunge un listener di eventi al pulsante CTA, ingrandendo l&#39;immagine al passaggio del mouse.
1. Aggiunge classi CSS semantiche agli elementi del blocco, utili per l’integrazione dei sistemi di progettazione CSS esistenti.
1. Aggiunge una classe CSS speciale ai paragrafi che iniziano con `Terms and conditions:`.

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="Nome del file dell’esempio di codice riportato di seguito."}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## Blocca CSS

Se hai creato un `teaser.css` nel [capitolo precedente](./7a-block-css.md), eliminalo o rinominalo `teaser.css.bak`, poiché questo capitolo implementa CSS diversi per il blocco teaser.

Crea un file `teaser.css` nella cartella del blocco. Questo file contiene il codice CSS che assegna lo stile al blocco. Questo codice CSS esegue il targeting degli elementi del blocco e delle specifiche classi CSS di semantica aggiunte dal JavaScript in `teaser.js`.

Gli elementi nudi possono ancora essere formattati direttamente o con le classi CSS applicate personalizzate. Per blocchi più complessi, l’applicazione di classi CSS semantiche può contribuire a rendere il CSS più comprensibile e gestibile, soprattutto quando si lavora con team più grandi per periodi di tempo più lunghi.

[Come prima](./7a-block-css.md#develop-a-block-with-css), assegna il CSS a `.block.teaser` utilizzando [la nidificazione CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) per evitare conflitti con altri blocchi.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Nome del file dell’esempio di codice riportato di seguito."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;

    /* The teaser image */
    & .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        & .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;
        }
    }

    /* The teaser text content */
    & .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        & .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        & .title::after {
            border-bottom: 0;
        }

        & p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        }

        & p.terms-and-conditions {
            font-size: var(--body-font-size-xs);
            color: var(--secondary-color);
            padding: .5rem 1rem;
            font-style: italic;
            border: solid var(--light-color);
            border-width: 0 0 0 10px;
        }

        /* Add underlines to links in the text */
        & a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        & .button-container {
            margin: 0;
            padding: 0;
        }

        & .button {   
            background-color: var(--primary-color);
            border-radius: 0;
            color: var(--dark-color);
            font-size: var(--body-font-size-xs);
            font-weight: bold;
            padding: 1em 2.5em;
            margin: 0;
            text-transform: uppercase;
        }
    }

    & .zoom {
        transform: scale(1.1);
    }
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## Aggiungi termini e condizioni

L&#39;implementazione di cui sopra aggiunge il supporto per paragrafi con stili speciali che iniziano con il testo `Terms and conditions:`. Per convalidare questa funzionalità, in Universal Editor aggiorna il contenuto di testo del blocco teaser in modo da includere termini e condizioni.

Segui i passaggi descritti in [creare un blocco](./6-author-block.md) e modifica il testo in modo da includere un paragrafo **termini e condizioni** alla fine:

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

Verifica che nell’ambiente di sviluppo locale venga eseguito il rendering del paragrafo con lo stile termini e condizioni. Ricorda che queste modifiche al codice non vengono applicate nell&#39;editor universale finché non vengono [inviate a un ramo su GitHub](#preview-in-universal-editor) per il quale è stato configurato l&#39;utilizzo dell&#39;editor universale.

## Anteprima di sviluppo

Con l’aggiunta di CSS e JavaScript, l’ambiente di sviluppo locale della CLI dell’AEM ricarica a caldo le modifiche, consentendo una visualizzazione rapida e semplice dell’impatto del codice sul blocco. Passa il puntatore del mouse sul CTA e verifica che l’immagine del teaser si ingrandisca e si ingrandisca.

![Anteprima di sviluppo locale del teaser tramite CSS e JS](./assets/7b-block-js-css/local-development-preview.png)

## Illustra il codice

Assicurati di [collegare frequentemente](./3-local-development-environment.md#linting) le modifiche al codice per mantenerlo pulito e coerente. L&#39;linting regolare consente di risolvere i problemi in anticipo, riducendo il tempo di sviluppo complessivo. Ricorda che non puoi unire il tuo lavoro di sviluppo nel ramo `main` finché non saranno stati risolti tutti i problemi di linting.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## Anteprima nell’editor universale

Per visualizzare le modifiche nell’editor universale dell’AEM, aggiungili, esegui il commit e inviali al ramo dell’archivio Git utilizzato dall’editor universale. In questo modo l’implementazione del blocco non interrompe l’esperienza di authoring.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

È ora possibile visualizzare in anteprima le modifiche nell&#39;editor universale quando si aggiunge il parametro di query `?ref=teaser`.

![Teaser nell&#39;editor universale](./assets/7b-block-js-css/universal-editor-preview.png)
