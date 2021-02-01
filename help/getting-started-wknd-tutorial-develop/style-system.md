---
title: Sviluppo con il sistema di stili
seo-title: Sviluppo con il sistema di stili
description: Scopri come implementare singoli stili e riutilizzare i componenti core con  Experience Manager  Style System. Questa esercitazione illustra lo sviluppo di Style System per estendere i componenti core con CSS specifici del marchio e configurazioni di criteri avanzati dell'Editor modelli.
sub-product: sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1996'
ht-degree: 1%

---


# Sviluppo con il sistema di stili {#developing-with-the-style-system}

Scopri come implementare singoli stili e riutilizzare i componenti core con  Experience Manager  Style System. Questa esercitazione illustra lo sviluppo di Style System per estendere i componenti core con CSS specifici del marchio e configurazioni di criteri avanzati dell&#39;Editor modelli.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

Si consiglia inoltre di consultare le [librerie lato client e l&#39;esercitazione sul flusso di lavoro front-end](client-side-libraries.md) per comprendere le basi delle librerie lato client e i vari strumenti front-end incorporati nel progetto AEM.

### Progetto iniziale

>[!NOTE]
>
> Se il capitolo precedente è stato completato correttamente, è possibile riutilizzare il progetto e ignorare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Estrarre il ramo `tutorial/style-system-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzate AEM 6.5 o 6.4, aggiungete il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) o estrarre il codice localmente passando al ramo `tutorial/style-system-solution`.

## Obiettivo

1. Scoprite come utilizzare il sistema di stile per applicare CSS specifico del marchio a AEM componenti core.
1. Scopri la notazione BEM e come utilizzarla per definire con attenzione gli stili di ambito.
1. Applica configurazioni di criteri avanzate con Modelli modificabili.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo utilizzeremo la funzione [Style System](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) per creare varianti dei componenti **Title** e **Text** utilizzati nella pagina Articolo.

![Stili disponibili per il titolo](assets/style-system/styles-added-title.png)

*Stile sottolineato disponibile per il componente Titolo*

## Sfondo {#background}

[Style System](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/components/style-system.html) consente agli sviluppatori e agli editor di modelli di creare più varianti visive di un componente. Gli autori possono quindi scegliere lo stile da usare per la composizione di una pagina. Nel resto dell&#39;esercitazione verrà utilizzato lo Style System per ottenere diversi stili univoci, sfruttando i componenti core in un approccio basato su codice basso.

L’idea generale del sistema di stile è che gli autori possono scegliere vari stili per l’aspetto di un componente. Gli &quot;stili&quot; sono supportati da ulteriori classi CSS inserite nel div esterno di un componente. Nelle librerie client le regole CSS vengono aggiunte in base a queste classi di stile in modo che il componente cambi aspetto.

È possibile trovare la [documentazione dettagliata per Style System qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html). C&#39;è anche un ottimo [video tecnico per capire il sistema Style](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Stile sottolineatura - Titolo {#underline-style}

Il componente [Titolo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html) è stato proxy nel progetto in `/apps/wknd/components/title` come parte del modulo **ui.apps**. Gli stili predefiniti degli elementi Titolo (`H1`, `H2`, `H3`...) sono già stati implementati nel modulo **ui.frontend**.

Le [progettazioni di articoli WKND](assets/pages-templates/wknd-article-design.xd) contengono uno stile univoco per il componente Titolo con una sottolineatura. Anziché creare due componenti o modificare la finestra di dialogo dei componenti, il sistema di stile può essere utilizzato per consentire agli autori di aggiungere uno stile di sottolineatura.

![Stile sottolineatura - Componente titolo](assets/style-system/title-underline-style.png)

### Marcatura titolo  Inspect

Come sviluppatore front-end, il primo passo per definire lo stile di un componente core consiste nel comprendere il codice generato dal componente.

1. Aprite un nuovo browser e visualizzate il componente Titolo sul sito AEM Libreria componenti core: [https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html)

1. Di seguito è riportata la marcatura per il componente Titolo:

   ```html
   <div class="cmp-title">
       <h1 class="cmp-title__text">Lorem Ipsum</h1>
   </div>
   ```

   Notazione BEM del componente Titolo:

   ```plain
   BLOCK cmp-title
       ELEMENT cmp-title__text
   ```

1. Il sistema Style aggiunge una classe CSS al div esterno che circonda il componente. Di conseguenza, il markup a cui verrà indirizzato il targeting sarà simile a quanto segue:

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-title">
           <h1 class="cmp-title__text">Lorem Ipsum</h1>
       </div>
   </div>
   ```

### Implementa lo stile sottolineatura - ui.frontend

Quindi, implementate lo stile Sottolineato utilizzando il modulo **ui.frontend** del nostro progetto. Verrà utilizzato il server di sviluppo webpack fornito con il modulo **ui.frontend** per visualizzare in anteprima gli stili *prima di* distribuire in un&#39;istanza locale di AEM.

1. Avviare il server di sviluppo webpack eseguendo il comando seguente dall&#39;interno del modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

   Questo dovrebbe aprire un browser in [http://localhost:8080](Http://localhost:8080).

   >[!NOTE]
   >
   > Se le immagini appaiono interrotte, accertatevi che il progetto iniziale sia stato distribuito in un&#39;istanza locale di AEM (in esecuzione sulla porta 4502) e che il browser utilizzato abbia eseguito l&#39;accesso anche all&#39;istanza AEM locale.

   ![Server di sviluppo webpack](assets/style-system/static-webpack-server.png)

1. Nell&#39;IDE aprire il file `index.html` situato in: `ui.frontend/src/main/webpack/static/index.html`. Si tratta del markup statico utilizzato dal server di sviluppo webpack.
1. In `index.html` trovare un&#39;istanza del componente Titolo a cui aggiungere lo stile sottolineato ricercando nel documento *cmp-title*. Scegliete il componente Titolo con il testo *&quot;Vans off the Wall Skatepark&quot;* (riga 218). Aggiungete la classe `cmp-title--underline` al div circostante:

   ```diff
   - <div class="title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-title--underline title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;title-8bea562fa0&#34;:{&#34;@type&#34;:&#34;wknd/components/title&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T18:54:20Z&#34;,&#34;dc:title&#34;:&#34;Vans Off the Wall&#34;}}" id="title-8bea562fa0" class="cmp-title">
            <h2 class="cmp-title__text">Vans Off the Wall</h2>
        </div>
    </div>
   ```

1. Tornate al browser e verificate che la classe supplementare sia riflessa nella marcatura.
1. Tornate al modulo **ui.frontend** e aggiornate il file `title.scss` che si trova in: `ui.frontend/src/main/webpack/components/_title.scss`:

   ```css
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
   >È consigliabile estendere sempre gli stili in modo stretto al componente di destinazione. In questo modo gli stili aggiuntivi non influiscono sulle altre aree della pagina.
   >
   >Tutti i componenti core aderiscono alla **[notazione BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. È consigliabile eseguire il targeting della classe CSS esterna quando si crea uno stile predefinito per un componente. Un&#39;altra procedura consigliata consiste nel eseguire il targeting dei nomi delle classi specificati dalla notazione BEM dei componenti core invece che degli elementi HTML.

1. Tornate nuovamente nel browser e vedrete lo stile Sottolineato aggiunto:

   ![Stile sottolineato visibile nel server di dev del webpack](assets/style-system/underline-implemented-webpack.png)

1. Arrestate il server di sviluppo del webpack.

### Aggiunta di un criterio di titolo

Successivamente è necessario aggiungere un nuovo criterio per i componenti Titolo per consentire agli autori di contenuti di scegliere lo stile Sottolineato da applicare a componenti specifici. A tal fine, si utilizza l&#39;Editor modelli in AEM.

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Andate al modello **Article Page** che si trova in: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. In modalità **Struttura**, nella **Contenitore di layout** principale, selezionare l&#39;icona **Policy** accanto al componente **Titolo** elencato in *Componenti consentiti*:

   ![Configurazione criteri di titolo](assets/style-system/article-template-title-policy-icon.png)

1. Crea un nuovo criterio per il componente Titolo con i seguenti valori:

   *Titolo criterio **:  **Titolo WKND**

   *Proprietà*  >  *Scheda*  Stili >  *Aggiungi un nuovo stile*

   **Sottolineato** :  `cmp-title--underline`

   ![Configurazione dei criteri di stile per il titolo](assets/style-system/title-style-policy.png)

   Fare clic su **Fine** per salvare le modifiche al criterio Titolo.

   >[!NOTE]
   >
   > Il valore `cmp-title--underline` corrisponde alla classe CSS di cui abbiamo eseguito il targeting in precedenza durante lo sviluppo nel modulo **ui.frontend**.

### Applica stile sottolineatura

Infine, come autore, possiamo scegliere di applicare lo stile sottolineato ad alcuni componenti titolo.

1. Andate all&#39;articolo **La Skateparks** nell&#39;editor AEM Sites  all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In modalità **Modifica**, scegliete un componente Titolo. Fare clic sull&#39;icona **pennello** e selezionare lo stile **Sottolineato**:

   ![Applica stile sottolineatura](assets/style-system/apply-underline-style-title.png)

   In qualità di autore, potete attivare/disattivare lo stile.

1. Fare clic sull&#39;icona **Informazioni pagina** > **Visualizza come pubblicato** per ispezionare la pagina all&#39;esterno dell&#39;editor AEM.

   ![Visualizza come pubblicato](assets/style-system/view-as-published.png)

   Utilizzate gli strumenti di sviluppo del browser per verificare che la marcatura intorno al componente Titolo sia applicata alla classe CSS `cmp-title--underline`.

## Stile blocco preventivo - Testo {#text-component}

Ripetere quindi i passaggi simili per applicare uno stile univoco al [componente di testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Il componente Testo è stato proxy nel progetto in `/apps/wknd/components/text` come parte del modulo **ui.apps**. Gli stili predefiniti degli elementi di paragrafo sono già stati implementati in **ui.frontend**.

Le [progettazioni di articoli WKND](assets/pages-templates/wknd-article-design.xd) contengono uno stile univoco per il componente Testo con un blocco di virgolette:

![Stile blocco preventivo - Componente testo](assets/style-system/quote-block-style.png)

###  Inspect Text Component Markup

Anche in questo caso, verrà esaminata la marcatura del componente Testo.

1. Rivedete la marcatura per il componente Testo in: [https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)

1. Di seguito è riportata la marcatura per il componente Testo:

   ```html
   <div class="text">
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

   Notazione BEM del componente Testo:

   ```plain
   BLOCK cmp-text
       ELEMENT
   ```

1. Il sistema Style aggiunge una classe CSS al div esterno che circonda il componente. Di conseguenza, il markup a cui verrà indirizzato il targeting sarà simile a quanto segue:

   ```html
   <div class="text STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

### Implementa lo stile del blocco di quote - ui.frontend

Successivamente verrà implementato lo stile Blocco preventivo utilizzando il modulo **ui.frontend** del nostro progetto.

1. Avviare il server di sviluppo webpack eseguendo il comando seguente dall&#39;interno del modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. Nell&#39;IDE, aprire il file `index.html` che si trova in: `ui.frontend/src/main/webpack/static/index.html`.
1. In `index.html` trovare un&#39;istanza del componente di testo cercando il testo *&quot;Jacob Wester&quot;* (riga 210). Aggiungete la classe `cmp-text--quote` al div circostante:

   ```diff
   - <div class="text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-text--quote text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;text-a15f39a83a&#34;:{&#34;@type&#34;:&#34;wknd/components/text&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T00:23:27Z&#34;,&#34;xdm:text&#34;:&#34;&lt;blockquote>&amp;quot;There is no better place to shred then Los Angeles.”&lt;/blockquote>\r\n&lt;p>- Jacob Wester, Pro Skater&lt;/p>\r\n&#34;}}" id="text-a15f39a83a" class="cmp-text">
            <blockquote>&quot;There is no better place to shred then Los Angeles.”</blockquote>
            <p>- Jacob Wester, Pro Skater</p>
        </div>
    </div>
   ```

1. Aggiorna il file `text.scss` che si trova in: `ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
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
   > In questo caso, gli elementi HTML non elaborati sono interessati dagli stili. Questo perché il componente Testo fornisce un editor Rich Text per gli autori di contenuti. La creazione di stili direttamente rispetto ai contenuti RTE deve essere effettuata con attenzione ed è ancora più importante definire gli stili in modo più preciso.

1. Tornate nuovamente nel browser e vedrete lo stile del blocco Quote aggiunto:

   ![Stile blocco preventivo visibile nel server dev webpack](assets/style-system/quoteblock-implemented-webpack.png)

1. Arrestate il server di sviluppo del webpack.

### Aggiungere un criterio di testo

Aggiungere quindi un nuovo criterio per i componenti Testo.

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Andate alla **pagina articolo Modello**, che si trova in: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)).

1. In modalità **Struttura**, nella **Contenitore di layout** principale, selezionare l&#39;icona **Policy** accanto al componente **Testo** elencato in *Componenti consentiti*:

   ![Configurazione criteri di testo](assets/style-system/article-template-text-policy-icon.png)

1. Aggiornare il criterio del componente Testo con i seguenti valori:

   *Titolo criterio **:  **Testo contenuto**

   *Plugins* >  *Stili*  paragrafo >  *Abilita stili paragrafo*

   *Scheda*  Stili >  *Aggiungi un nuovo stile*

   **Blocco**  preventivo:  `cmp-text--quote`

   ![Criterio componente testo](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Criterio componente testo 2](assets/style-system/text-policy-enable-quotestyle.png)

   Fare clic su **Fine** per salvare le modifiche al criterio Testo.

### Applica stile blocco preventivo

1. Andate all&#39;articolo **La Skateparks** nell&#39;editor AEM Sites  all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In modalità **Modifica**, scegliete un componente Testo. Modificate il componente per includere un elemento di offerta:

   ![Configurazione del componente di testo](assets/style-system/configure-text-component.png)

1. Selezionate il componente di testo e fate clic sull&#39;icona **pennello**, quindi selezionate lo stile **Blocco preventivi**:

   ![Applica stile blocco preventivo](assets/style-system/quote-block-style-applied.png)

   In qualità di autore, potete attivare/disattivare lo stile.

## Larghezza fissa - Contenitore (Bonus) {#layout-container}

I componenti Contenitore sono stati utilizzati per creare la struttura di base del Modello pagina articolo e fornire le zone di rilascio per consentire agli autori di contenuti di aggiungere contenuto a una pagina. I contenitori possono anche sfruttare lo Style System, fornendo agli autori dei contenuti ulteriori opzioni per la progettazione dei layout.

Il **Contenitore principale** del modello Pagina articolo contiene i due contenitori modificabili e ha una larghezza fissa.

![Contenitore principale](assets/style-system/main-container-article-page-template.png)

*Contenitore principale nel modello* pagina articolo.

Il criterio del **Contenitore principale** imposta l&#39;elemento predefinito come `main`:

![Criteri contenitore principale](assets/style-system/main-container-policy.png)

Il CSS che rende fisso il **Contenitore principale** è impostato nel modulo **ui.frontend** in `ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

Invece di impostare come destinazione l&#39;elemento HTML `main`, il sistema di stile può essere utilizzato per creare uno stile **Larghezza fissa** come parte del criterio Contenitore. Il sistema di stile potrebbe offrire agli utenti la possibilità di alternare tra contenitori **Larghezza fissa** e **Larghezza fluida**.

1. **Bonus Challenge**  - utilizzare le lezioni apprese dagli esercizi precedenti e utilizzare il sistema di stile per implementare uno stile di  **larghezza** fissa e  **fluido** per il componente Contenitore.

## Congratulazioni! {#congratulations}

Congratulazioni, la pagina dell&#39;articolo è quasi completamente stilizzata e avete acquisito esperienza pratica utilizzando il sistema di stile AEM.

### Passaggi successivi {#next-steps}

Scopri i passaggi end-to-end per creare un [componente AEM personalizzato](custom-component.md) che visualizza il contenuto creato in una finestra di dialogo ed esplora lo sviluppo di un modello Sling per racchiudere logica aziendale che compone l&#39;HTL del componente.

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in corrispondenza del blocco Git `tutorial/style-system-solution`.

1. Duplicare il repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `tutorial/style-system-solution`.
