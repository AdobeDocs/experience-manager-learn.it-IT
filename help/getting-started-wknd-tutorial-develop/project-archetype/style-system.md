---
title: Sviluppo con il sistema di stili
description: Scopri come implementare singoli stili e riutilizzare i Componenti core utilizzando il sistema di stili di Experience Manager. Questa esercitazione tratta lo sviluppo per il sistema di stili per estendere i componenti core con CSS specifici per il brand e configurazioni di policy avanzate dell’Editor modelli.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4128
mini-toc-levels: 1
thumbnail: 30386.jpg
doc-type: Tutorial
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
duration: 358
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# Sviluppo con il sistema di stili {#developing-with-the-style-system}

Scopri come implementare singoli stili e riutilizzare i Componenti core utilizzando il sistema di stili di Experience Manager. Questa esercitazione tratta lo sviluppo per il sistema di stili per estendere i componenti core con CSS specifici per il brand e configurazioni di policy avanzate dell’Editor modelli.

## Prerequisiti {#prerequisites}

Esaminare gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

È inoltre consigliabile rivedere l&#39;esercitazione [Librerie lato client e flusso di lavoro front-end](client-side-libraries.md) per comprendere le nozioni di base delle librerie lato client e dei vari strumenti front-end incorporati nel progetto AEM.

### Progetto iniziale

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per estrarre il progetto iniziale.

Consulta il codice della riga di base su cui si basa l’esercitazione:

1. Controlla il ramo `tutorial/style-system-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. Implementa la base di codice in un’istanza AEM locale utilizzando le abilità Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) o estrarre il codice localmente passando al ramo `tutorial/style-system-solution`.

## Obiettivo

1. Scopri come utilizzare il sistema di stili per applicare CSS specifici per il brand ai componenti core di AEM.
1. Scopri la notazione BEM e come utilizzarla per definire con attenzione gli stili.
1. Applicare configurazioni di criteri avanzate con modelli modificabili.

## Cosa intendi creare {#what-build}

In questo capitolo viene utilizzata la funzionalità [Sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=it) per creare varianti dei componenti **Titolo** e **Testo** utilizzati nella pagina dell&#39;articolo.

![Stili disponibili per il titolo](assets/style-system/styles-added-title.png)

*Stile sottolineato disponibile per il componente Titolo*

## Informazioni di base {#background}

Il [sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=it) consente agli sviluppatori e agli editor di modelli di creare più varianti visive di un componente. Gli autori possono quindi decidere quale stile utilizzare durante la composizione di una pagina. Il sistema di stili viene utilizzato in tutto il resto dell’esercitazione per ottenere diversi stili univoci utilizzando i Componenti core in un approccio a basso codice.

L’idea generale del sistema di stili è che gli autori possano scegliere vari stili per definire l’aspetto di un componente. Gli &quot;stili&quot; sono supportati da classi CSS aggiuntive che vengono inserite nell’elemento div esterno di un componente. Nelle librerie client vengono aggiunte regole CSS basate su queste classi di stile in modo che il componente cambi aspetto.

Puoi trovare [la documentazione dettagliata per il sistema di stili qui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=it). C&#39;è anche un ottimo [video tecnico per comprendere il sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html?lang=it).

## Stile sottolineatura - Titolo {#underline-style}

Il componente [Titolo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html?lang=it) è stato aggiunto al progetto in `/apps/wknd/components/title` come parte del modulo **ui.apps**. Gli stili predefiniti degli elementi Titolo (`H1`, `H2`, `H3`...) sono già stati implementati nel modulo **ui.frontend**.

Le progettazioni dell&#39;articolo [WKND](assets/pages-templates/wknd-article-design.xd) contengono uno stile univoco per il componente Titolo con una sottolineatura. Invece di creare due componenti o modificare la finestra di dialogo del componente, è possibile utilizzare il sistema di stili per consentire agli autori di aggiungere uno stile sottolineato.

![Stile sottolineato - Componente titolo](assets/style-system/title-underline-style.png)

### Aggiungi un criterio titolo

Aggiungiamo un criterio per i componenti Titolo per consentire agli autori di contenuto di scegliere lo stile Sottolineato da applicare a componenti specifici. Questa operazione viene eseguita utilizzando l’Editor modelli in AEM.

1. Passa al modello **Pagina articolo** da: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. In modalità **Struttura**, nel **Contenitore layout** principale, selezionare l&#39;icona **Criteri** accanto al componente **Titolo** elencato in *Componenti consentiti*:

   ![Configurazione criteri titolo](assets/style-system/article-template-title-policy-icon.png)

1. Crea un criterio per il componente Titolo con i seguenti valori:

   *Titolo criterio&#42;*: **Titolo WKND**

   *Proprietà* > *Scheda Stili* > *Aggiungi un nuovo stile*

   **Sottolineato**: `cmp-title--underline`

   ![Configurazione dei criteri di stile per il titolo](assets/style-system/title-style-policy.png)

   Fai clic su **Fine** per salvare le modifiche ai criteri del titolo.

   >[!NOTE]
   >
   > Il valore `cmp-title--underline` popola la classe CSS sull&#39;elemento div esterno del markup HTML del componente.

### Applicare lo stile Sottolineato

In qualità di autore, applichiamo lo stile sottolineato a determinati componenti titolo.

1. Passa all&#39;articolo **La Skateparks** nell&#39;editor di AEM Sites all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In modalità **Modifica**, scegli un componente Titolo. Fai clic sull&#39;icona **pennello** e seleziona lo stile **Sottolineato**:

   ![Applica stile sottolineatura](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > A questo punto, non si verifica alcuna modifica visibile poiché lo stile `underline` non è stato implementato. Nell’esercizio successivo, questo stile viene implementato.

1. Fai clic sull&#39;icona **Informazioni pagina** > **Visualizza come pubblicato** per controllare la pagina all&#39;esterno dell&#39;editor di AEM.
1. Utilizzare gli strumenti di sviluppo del browser per verificare che al markup del componente Titolo sia applicata la classe CSS `cmp-title--underline` al div esterno.

   ![Div con classe sottolineata applicata](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### Implementare lo stile Sottolineato - ui.frontend

Quindi, implementa lo stile Sottolineato utilizzando il modulo **ui.frontend** del progetto AEM. Viene utilizzato il server di sviluppo Webpack fornito con il modulo **ui.frontend** per visualizzare in anteprima gli stili *prima* della distribuzione in un&#39;istanza locale di AEM.

1. Avvia il processo `watch` dal modulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   Verrà avviato un processo che monitora le modifiche nel modulo `ui.frontend` e sincronizza le modifiche con l&#39;istanza di AEM.


1. Restituire l&#39;IDE e aprire il file `_title.scss` da: `ui.frontend/src/main/webpack/components/_title.scss`.
1. Introdurre una nuova regola destinata alla classe `cmp-title--underline`:

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
   >È consigliabile definire sempre con precisione l’ambito degli stili del componente di destinazione. In questo modo gli stili aggiuntivi non influiranno su altre aree della pagina.
   >
   >Tutti i Componenti core sono conformi alla notazione **[BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. È consigliabile eseguire il targeting della classe CSS esterna durante la creazione di uno stile predefinito per un componente. Un’altra best practice è quella di eseguire il targeting dei nomi di classe specificati dalla notazione BEM dei Componenti core, anziché degli elementi HTML.

1. Torna al browser e alla pagina AEM. Dovresti notare che è stato aggiunto lo stile Sottolineato:

   ![Stile sottolineato visibile nel server di sviluppo Webpack](assets/style-system/underline-implemented-webpack.png)

1. Nell&#39;editor di AEM, ora dovresti essere in grado di attivare e disattivare lo stile **Sottolineato** e vedere che le modifiche si riflettono visivamente.

## Stile blocco preventivo - Testo {#text-component}

Ripetere quindi i passaggi simili per applicare uno stile univoco al [componente testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html?lang=it). Il componente Testo è stato aggiunto come proxy al progetto in `/apps/wknd/components/text` come parte del modulo **ui.apps**. Gli stili predefiniti degli elementi paragrafo sono già stati implementati in **ui.frontend**.

Le progettazioni dell&#39;articolo [WKND](assets/pages-templates/wknd-article-design.xd) contengono uno stile univoco per il componente Testo con un blocco di virgolette:

![Stile blocco preventivo - Componente testo](assets/style-system/quote-block-style.png)

### Aggiungi un criterio di testo

Quindi aggiungi un criterio per i componenti Testo.

1. Passa al **modello pagina articolo** da: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. In modalità **Struttura**, nel **Contenitore layout** principale, selezionare l&#39;icona **Criteri** accanto al componente **Testo** elencato in *Componenti consentiti*:

   ![Configurazione criteri di testo](assets/style-system/article-template-text-policy-icon.png)

1. Aggiorna il criterio del componente Testo con i seguenti valori:

   *Titolo criterio&#42;*: **Testo contenuto**

   *Plug-in* > *Stili paragrafo* > *Abilita stili paragrafo*

   *Scheda Stili* > *Aggiungi un nuovo stile*

   **Blocco preventivo**: `cmp-text--quote`

   ![Criterio componente testo](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Criteri componente testo 2](assets/style-system/text-policy-enable-quotestyle.png)

   Fai clic su **Fine** per salvare le modifiche ai criteri di testo.

### Applicare lo stile di blocco del preventivo

1. Passa all&#39;articolo **La Skateparks** nell&#39;editor di AEM Sites all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In modalità **Modifica**, scegli un componente Testo. Modifica il componente per includere un elemento preventivo:

   ![Configurazione componente testo](assets/style-system/configure-text-component.png)

1. Seleziona il componente di testo e fai clic sull&#39;icona **pennello**, quindi seleziona lo stile **Blocco preventivo**:

   ![Applicare lo stile di blocco del preventivo](assets/style-system/quote-block-style-applied.png)

1. Utilizza gli strumenti per sviluppatori del browser per controllare il markup. Dovresti vedere che il nome della classe `cmp-text--quote` è stato aggiunto all&#39;elemento div esterno del componente:

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### Implementare lo stile di blocco del preventivo - ui.frontend

Ora implementiamo lo stile Blocco preventivo utilizzando il modulo **ui.frontend** del progetto AEM.

1. Se non è già in esecuzione, avviare il processo `watch` dal modulo **ui.frontend**:

   ```shell
   $ npm run watch
   ```

1. Aggiorna il file `text.scss` da: `ui.frontend/src/main/webpack/components/_text.scss`:

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
   > In questo caso, gli elementi HTML non elaborati sono interessati dagli stili. Questo perché il componente Testo fornisce un editor Rich Text per gli autori di contenuto. La creazione di stili direttamente rispetto al contenuto dell’editor Rich Text deve essere eseguita con attenzione ed è ancora più importante definire in modo preciso gli stili.

1. Torna nuovamente al browser e osserva che è stato aggiunto lo stile di blocco Preventivo:

   ![Stile blocco preventivo visibile](assets/style-system/quoteblock-implemented.png)

1. Arresta il server di sviluppo Webpack.

## Larghezza fissa - Contenitore (bonus) {#layout-container}

I componenti Contenitore sono stati utilizzati per creare la struttura di base del modello per pagina articolo e fornire le zone di rilascio per consentire agli autori di contenuto di aggiungere contenuto a una pagina. I contenitori possono inoltre utilizzare il sistema di stili, fornendo agli autori di contenuto ulteriori opzioni per la progettazione dei layout.

Il **contenitore principale** del modello Pagina articolo contiene i due contenitori modificabili e ha una larghezza fissa.

![Contenitore principale](assets/style-system/main-container-article-page-template.png)

*Contenitore principale nel modello pagina articolo*.

Il criterio del **contenitore principale** imposta l&#39;elemento predefinito come `main`:

![Criteri contenitore principale](assets/style-system/main-container-policy.png)

Il CSS che corregge il contenitore principale **contenitore** è impostato nel modulo **ui.frontend** in `ui.frontend/src/main/webpack/site/styles/container_main.scss`:

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

Invece di eseguire il targeting dell&#39;elemento HTML `main`, è possibile utilizzare il sistema di stili per creare uno stile **a larghezza fissa** come parte del criterio Contenitore. Il sistema di stili potrebbe consentire agli utenti di passare da **a larghezza fissa** a **a larghezza fluida**.

1. **Sfida bonus**: utilizza gli insegnamenti appresi dagli esercizi precedenti e utilizza il sistema di stili per implementare gli stili **Larghezza fissa** e **Larghezza fluida** per il componente Contenitore.

## Congratulazioni. {#congratulations}

Congratulazioni, la pagina dell’articolo è quasi formattata e hai acquisito un’esperienza pratica utilizzando il sistema di stili di AEM.

### Passaggi successivi {#next-steps}

Scopri i passaggi end-to-end per creare un [componente AEM personalizzato](custom-component.md) che visualizza il contenuto creato in una finestra di dialogo ed esplora lo sviluppo di un modello Sling per incapsulare la logica di business che popola l&#39;HTL del componente.

Visualizza il codice finito in [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente in nel ramo Git `tutorial/style-system-solution`.

1. Clona l&#39;archivio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Controllare il ramo `tutorial/style-system-solution`.
