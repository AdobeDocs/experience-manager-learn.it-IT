---
title: Sviluppo con il sistema di stili
seo-title: Developing with the Style System
description: Scopri come implementare singoli stili e riutilizzare i componenti core utilizzando il sistema di stili di Experience Manager. Questa esercitazione illustra lo sviluppo per il sistema di stili per estendere i componenti core con CSS specifici per il brand e configurazioni avanzate di criteri nell’Editor modelli.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 2%

---

# Sviluppo con il sistema di stili {#developing-with-the-style-system}

Scopri come implementare singoli stili e riutilizzare i componenti core utilizzando il sistema di stili di Experience Manager. Questa esercitazione illustra lo sviluppo per il sistema di stili per estendere i componenti core con CSS specifici per il brand e configurazioni avanzate di criteri nell’Editor modelli.

## Prerequisiti {#prerequisites}

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

Si consiglia inoltre di rivedere il [Librerie lato client e flusso di lavoro front-end](client-side-libraries.md) esercitazione per comprendere i fondamenti delle librerie lato client e i vari strumenti front-end incorporati nel progetto AEM.

### Progetto iniziale

>[!NOTE]
>
> Se hai completato con successo il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Consulta la sezione `tutorial/style-system-start` ramo [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi la variabile `classic` su qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) o controlla il codice localmente passando al ramo `tutorial/style-system-solution`.

## Obiettivo

1. Scopri come utilizzare il sistema di stili per applicare CSS specifici per il brand a AEM componenti core.
1. Scopri la notazione BEM e come utilizzarla per definire con attenzione gli stili.
1. Applicare configurazioni di criteri avanzate con Modelli modificabili.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo utilizzeremo il [Funzionalità del sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) per creare varianti di **Titolo** e **Testo** componenti utilizzati nella pagina Articolo.

![Stili disponibili per il titolo](assets/style-system/styles-added-title.png)

*Stile sottolineatura disponibile per il componente Titolo*

## Informazioni di base {#background}

La [Sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) consente agli sviluppatori e agli editor di modelli di creare più varianti visive di un componente. Gli autori possono quindi a loro volta decidere quale stile utilizzare per la composizione di una pagina. Nel resto dell’esercitazione, sfrutteremo il sistema di stili per ottenere diversi stili univoci, sfruttando al contempo i componenti core in un approccio a basso codice.

L’idea generale del sistema di stili è che gli autori possono scegliere vari stili per l’aspetto di un componente. Gli &quot;stili&quot; sono supportati da classi CSS aggiuntive inserite nel div esterno di un componente. Nelle librerie client le regole CSS vengono aggiunte in base a queste classi di stile in modo che il componente cambi aspetto.

È possibile trovare [documentazione dettagliata su Sistema di stili qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html?lang=it). C&#39;è anche un grande [video tecnico per comprendere il sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Stile sottolineatura - Titolo {#underline-style}

La [Componente titolo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html) è stato oggetto di un proxy nel progetto `/apps/wknd/components/title` come parte del **ui.apps** modulo . Gli stili predefiniti degli elementi Titolo (`H1`, `H2`, `H3`...) sono già stati implementati nel **ui.frontend** modulo .

La [progettazioni di articoli WKND](assets/pages-templates/wknd-article-design.xd) contiene uno stile univoco per il componente Titolo con una sottolineatura. Invece di creare due componenti o modificare la finestra di dialogo del componente, il sistema di stili può essere utilizzato per consentire agli autori di aggiungere uno stile sottolineato.

![Stile sottolineatura - Componente titolo](assets/style-system/title-underline-style.png)

### Aggiungere un criterio per i titoli

Aggiungi un nuovo criterio per i componenti Titolo per consentire agli autori di contenuti di scegliere lo stile Sottolineato da applicare a componenti specifici. Questa operazione viene eseguita utilizzando l’Editor modelli in AEM.

1. Passa a **Pagina dell’articolo** modello situato in: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. In **Struttura** nella modalità principale **Contenitore di layout**, seleziona **Criterio** accanto all’icona **Titolo** componente elencato in *Componenti consentiti*:

   ![Configurazione dei criteri dei titoli](assets/style-system/article-template-title-policy-icon.png)

1. Crea un nuovo criterio per il componente Titolo con i seguenti valori:

   *Titolo criterio&#42;*: **Titolo WKND**

   *Proprietà* > *Scheda Stili* > *Aggiungi un nuovo stile*

   **Sottolineato** : `cmp-title--underline`

   ![Configurazione dei criteri di stile per il titolo](assets/style-system/title-style-policy.png)

   Fai clic su **Fine** per salvare le modifiche al criterio Titolo.

   >[!NOTE]
   >
   > Il valore `cmp-title--underline` compila la classe CSS sul div esterno del markup HTML del componente.

### Applica lo stile sottolineato

In qualità di autore, applica lo stile sottolineato a determinati componenti titolo.

1. Passa a **La Skatepark** nell’editor AEM Sites all’indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In **Modifica** scegli un componente Titolo . Fai clic sul pulsante **pennello** e seleziona la **Sottolineato** stile:

   ![Applica lo stile sottolineato](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > A questo punto non si verificherà alcuna modifica visibile come `underline` lo stile non è stato implementato. Nell’esercizio successivo questo stile viene implementato.

1. Fai clic sul pulsante **Informazioni pagina** icona > **Visualizza come pubblicato** per controllare la pagina all’esterno dell’editor AEM.
1. Utilizza gli strumenti per sviluppatori del browser per verificare che il markup intorno al componente Titolo abbia la classe CSS `cmp-title--underline` applicato al div esterno.

   ![Div con classe sottolineata applicata](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### Implementa lo stile di sottolineatura - ui.frontend

Quindi, implementa lo stile Sottolineato utilizzando **ui.frontend** modulo del nostro progetto. Useremo il server di sviluppo del webpack fornito in bundle con **ui.frontend** modulo per visualizzare in anteprima gli stili *prima* distribuzione in un&#39;istanza locale di AEM.

1. Avvia la `watch` dal **ui.frontend** modulo:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   Verrà avviato un processo per il monitoraggio delle modifiche nel `ui.frontend` e sincronizza le modifiche all&#39;istanza AEM.


1. Restituisci l’IDE e apri il file `_title.scss` situato a: `ui.frontend/src/main/webpack/components/_title.scss`.
1. Introdurre una nuova regola per il targeting delle `cmp-title--underline` Classe:

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >È consigliabile estendere sempre gli stili al componente di destinazione. In questo modo gli stili aggiuntivi non influiscono sulle altre aree della pagina.
   >
   >Tutti i componenti core aderiscono a **[Notazione BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. È consigliabile eseguire il targeting della classe CSS esterna durante la creazione di uno stile predefinito per un componente. Un’altra best practice consiste nell’eseguire il targeting dei nomi di classe specificati dalla notazione BEM del componente core anziché dagli elementi HTML.

1. Torna al browser e alla pagina AEM. Dovresti visualizzare lo stile Sottolineato aggiunto:

   ![Stile sottolineato visibile nel server di sviluppo del webpack](assets/style-system/underline-implemented-webpack.png)

1. Nell’editor AEM ora dovresti essere in grado di attivare e disattivare la funzione **Sottolineato** visualizza le modifiche visualizzate visivamente.

## Stile blocco preventivo - Testo {#text-component}

Quindi, ripeti passaggi simili per applicare uno stile univoco a [Componente testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Il componente Testo è stato proxy nel progetto in `/apps/wknd/components/text` come parte del **ui.apps** modulo . Gli stili predefiniti degli elementi paragrafo sono già stati implementati nel **ui.frontend**.

La [progettazioni di articoli WKND](assets/pages-templates/wknd-article-design.xd) contengono uno stile univoco per il componente Testo con un blocco di virgolette:

![Stile blocco preventivo - Componente testo](assets/style-system/quote-block-style.png)

### Aggiungere un criterio di testo

Aggiungere quindi un nuovo criterio per i componenti Testo .

1. Passa a **Modello pagina articolo** situato a: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. In **Struttura** nella modalità principale **Contenitore di layout**, seleziona **Criterio** accanto all’icona **Testo** componente elencato in *Componenti consentiti*:

   ![Configurazione dei criteri di testo](assets/style-system/article-template-text-policy-icon.png)

1. Aggiorna il criterio del componente Testo con i seguenti valori:

   *Titolo criterio&#42;*: **Testo contenuto**

   *Plug-in* > *Stili paragrafo* > *Abilitare gli stili di paragrafo*

   *Scheda Stili* > *Aggiungi un nuovo stile*

   **Blocco preventivo** : `cmp-text--quote`

   ![Criterio componente testo](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Criterio componente testo 2](assets/style-system/text-policy-enable-quotestyle.png)

   Fai clic su **Fine** per salvare le modifiche al criterio Testo.

### Applica lo stile del blocco del preventivo

1. Passa a **La Skatepark** nell’editor AEM Sites all’indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In **Modifica** scegli un componente Testo . Modifica il componente per includere un elemento del preventivo:

   ![Configurazione del componente testo](assets/style-system/configure-text-component.png)

1. Seleziona il componente testo e fai clic sul pulsante **pennello** e seleziona la **Blocco preventivo** stile:

   ![Applica lo stile del blocco del preventivo](assets/style-system/quote-block-style-applied.png)

1. Utilizza gli strumenti di sviluppo del browser per controllare il markup. Dovresti visualizzare il nome della classe `cmp-text--quote` è stato aggiunto al div esterno del componente:

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### Implementa lo stile del blocco del preventivo - ui.frontend

Ora implementeremo lo stile Blocco preventivo utilizzando **ui.frontend** modulo del nostro progetto.

1. Se non è già in esecuzione, avvia il `watch` dal **ui.frontend** modulo:

   ```shell
   $ npm run watch
   ```

1. Aggiornare il file `text.scss` situato a: `ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > In questo caso gli elementi HTML non elaborati sono interessati dagli stili. Questo perché il componente Testo fornisce un editor Rich Text per gli autori di contenuti. La creazione di stili direttamente rispetto ai contenuti RTE deve essere effettuata con attenzione ed è ancora più importante estendere gli stili.

1. Torna nuovamente al browser e dovresti visualizzare lo stile del blocco Preventivo aggiunto:

   ![Stile blocco preventivo visibile](assets/style-system/quoteblock-implemented.png)

1. Arrestare il server di sviluppo del webpack.

## Larghezza fissa - Contenitore (Bonus) {#layout-container}

I componenti contenitore sono stati utilizzati per creare la struttura di base del modello di pagina dell’articolo e forniscono agli autori dei contenuti le aree di rilascio per aggiungere contenuti a una pagina. I contenitori possono inoltre sfruttare il sistema di stili, fornendo agli autori dei contenuti ulteriori opzioni per la progettazione dei layout.

La **Contenitore principale** del modello Pagina articolo contiene i due contenitori modificabili dall’autore e ha una larghezza fissa.

![Contenitore principale](assets/style-system/main-container-article-page-template.png)

*Contenitore principale nel modello di pagina dell’articolo*.

La politica **Contenitore principale** imposta l’elemento predefinito come `main`:

![Criteri dei contenitori principali](assets/style-system/main-container-policy.png)

Il CSS che rende il **Contenitore principale** è impostato in **ui.frontend** modulo a `ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

Invece di eseguire il targeting `main` Elemento di HTML, il sistema di stili può essere utilizzato per creare un **Larghezza fissa** come parte del criterio Contenitore. Il sistema di stili potrebbe consentire agli utenti di passare da un sistema all’altro **Larghezza fissa** e **Larghezza fluida** contenitori.

1. **Sfida bonus** - utilizzare le lezioni tratte dagli esercizi precedenti e utilizzare il sistema di stili per implementare un **Larghezza fissa** e **Larghezza fluida** Stili per il componente Contenitore .

## Congratulazioni! {#congratulations}

Congratulazioni, la pagina dell’articolo è quasi completamente formattata e hai acquisito un’esperienza pratica utilizzando il sistema di stili AEM.

### Passaggi successivi {#next-steps}

Scopri i passaggi end-to-end per creare un [Componente AEM personalizzato](custom-component.md) visualizza il contenuto creato in una finestra di dialogo ed esplora lo sviluppo di un modello Sling per incapsulare la logica di business che popola l’HTL del componente.

Visualizza il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) o rivedi e distribuisci il codice localmente in sul blocco Git `tutorial/style-system-solution`.

1. Clona il [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) archivio.
1. Consulta la sezione `tutorial/style-system-solution` ramo.
