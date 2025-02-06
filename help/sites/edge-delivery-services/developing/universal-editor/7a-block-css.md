---
title: Sviluppare un blocco con CSS
description: Sviluppa un blocco con CSS per Edge Delivery Services, modificabile tramite l’Editor universale.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 14cda9d4-752b-4425-a469-8b6f283ce1db
source-git-commit: ecd3ce33204fa6f3f2c27ebf36e20ec26e429981
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Sviluppare un blocco con CSS

Lo stile dei blocchi nei Edge Delivery Services viene impostato tramite CSS. Il file CSS di un blocco viene memorizzato nella directory del blocco e ha lo stesso nome del blocco. Ad esempio, il file CSS per un blocco denominato `teaser` si trova in `blocks/teaser/teaser.css`.

Idealmente, un blocco dovrebbe avere solo CSS per lo stile, senza affidarsi a JavaScript per modificare il DOM o aggiungere classi CSS. La necessità di JavaScript dipende dalla [modellazione del contenuto](./5-new-block.md#block-model) del blocco e dalla sua complessità. Se necessario, è possibile aggiungere [il blocco JavaScript](./7b-block-js-css.md).

Utilizzando un approccio solo CSS, gli elementi HTML semantici (per lo più) nudi del blocco sono mirati e formattati.

## Blocca HTML

Per capire come assegnare uno stile a un blocco, rivedi innanzitutto il DOM esposto dai Edge Delivery Services, in quanto è ciò che è disponibile per lo stile. Il DOM può essere trovato esaminando il blocco servito dall&#39;ambiente di sviluppo locale della CLI dell&#39;AEM. Evita di utilizzare il DOM dell’editor universale, in quanto è leggermente diverso.

>[!BEGINTABS]

>[!TAB DOM a stile]

Di seguito è riportato il DOM del blocco teaser che è la destinazione per lo stile.

Osserva `<p class="button-container">...` che è [aumentato automaticamente](./4-website-branding.md#inferred-elements) come elemento dedotto da Edge Delivery Services JavaScript.

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

Per trovare lo stile DOM da applicare, apri la pagina con il blocco non formattato nell’ambiente di sviluppo locale, seleziona il blocco e controlla il DOM.

![DOM a blocchi Inspect](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]

## Blocca CSS

Crea un nuovo file CSS nella cartella del blocco, utilizzando il nome del blocco come nome del file. Ad esempio, per il blocco **teaser**, il file si trova in `/blocks/teaser/teaser.css`.

Questo file CSS viene caricato automaticamente quando il JavaScript del Edge Delivery Services rileva un elemento DOM nella pagina che rappresenta un blocco teaser.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Nome del file dell’esempio di codice riportato di seguito."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` using CSS nesting (https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in .6s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;

    /* The image is rendered to the first div in the block */
    & picture {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;

        & img {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
        }
    }

    /** 
    The teaser's text is rendered to the second (also the last) div in the block.

    These styles are scoped to the second (also the last) div in the block (.block.teaser > div:last-child).

    This div order can be used to target different styles to the same semantic elements in the block. 
    For example, if the block has two images, we could target the first image with `.block.teaser > div:first-child img`, 
    and the second image with `.block.teaser > div:nth-child(2) img`.
    **/
    & > div:last-child {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;

        /** 
        The following elements reside within `.block.teaser > div:last-child` and could be scoped as such, for example:

        .block.teaser > div:last-child p { .. }

        However since these element can only appear in the second/last div per our block's model, it's unnecessary to add this additional scope.
        **/

        /* Regardless of the authored heading level, we only want one style the heading */
        & h1,
        & h2,
        & h3,
        & h4,
        & h5,
        & h6 {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        & h1::after,
        & h2::after,
        & h3::after,
        & h4::after,
        & h5::after,
        & h6::after {
            border-bottom: 0;
        }

        & p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
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

## Anteprima di sviluppo

Poiché il CSS è scritto nel progetto di codice, il ricaricamento a caldo della CLI dell’AEM rappresenta le modifiche, rendendo facile e veloce la comprensione di come il CSS influisce sul blocco.

![Solo anteprima CSS](./assets/7a-block-css/local-development-preview.png)

## Illustra il codice

Assicurati di [collegare frequentemente](./3-local-development-environment.md#linting) le modifiche al codice per assicurarti che sia pulito e coerente. La colorazione consente di rilevare i problemi in anticipo e riduce il tempo di sviluppo complessivo. Ricorda che non puoi unire il tuo lavoro di sviluppo a `main` finché tutti i tuoi problemi di linting non saranno stati risolti.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:css
```

## Anteprima nell’editor universale

Per visualizzare le modifiche nell’editor universale dell’AEM, aggiungili, esegui il commit e inviali al ramo dell’archivio Git utilizzato dall’editor universale. Questo passaggio garantisce che l’implementazione del blocco non interrompa l’esperienza di authoring.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add CSS-only implementation for teaser block"
$ git push origin teaser
```

È ora possibile visualizzare in anteprima le modifiche nell&#39;editor universale quando si aggiunge il parametro di query `?ref=teaser`.

![Teaser nell&#39;editor universale](./assets/7a-block-css/universal-editor-preview.png)
