---
title: Informazioni sul codice per il sistema di stili AEM
description: Questo video illustra l’anatomia dei CSS (o LESS) e JavaScript utilizzati per assegnare uno stile al componente titolo di base di Adobe Experience Manager utilizzando il sistema di stili, nonché il modo in cui questi stili vengono applicati all’HTML e al DOM.
feature: Sistema di stili
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
topic: Sviluppo
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1148'
ht-degree: 2%

---


# Informazioni sul codice per il sistema di stili{#understanding-how-to-code-for-the-aem-style-system}

Questo video illustra l’anatomia dei CSS (o [!DNL LESS]) e JavaScript utilizzati per assegnare uno stile al componente titolo di base di Experience Manager utilizzando il sistema di stili, nonché il modo in cui questi stili vengono applicati all’HTML e al DOM.

>[!NOTE]
>
>Il sistema di stili AEM è stato introdotto con [AEM 6.3 SP1](https://helpx.adobe.com/it/experience-manager/6-3/release-notes/sp1-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593).
>
>Il video presuppone che il componente Titolo We.Retail sia stato aggiornato per ereditare da [Componenti core v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases).

## Informazioni sul codice per il sistema di stili {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

Il pacchetto AEM fornito (**technical-review.sites.style-system-1.0.0.zip**) installa lo stile del titolo dell&#39;esempio, i criteri di esempio per i componenti Contenitore di layout We.Retail e Titolo e una pagina di esempio.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

Di seguito è riportata la definizione [!DNL LESS] per lo stile di esempio trovato in:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Per coloro che preferiscono il CSS, sotto questo frammento di codice si trova il CSS in cui viene compilato questo [!DNL LESS].

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

I dati riportati sopra [!DNL LESS] vengono compilati in modo nativo per Experience Manager nel seguente CSS.

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### JavaScript {#example-javascript}

Il seguente JavaScript raccoglie e inserisce l’ultima data e l’ora di modifica della pagina corrente sotto il testo del titolo quando lo stile Esempio viene applicato al componente Titolo.

L’utilizzo di jQuery è facoltativo, così come le convenzioni di denominazione utilizzate.

Di seguito è riportata la definizione [!DNL LESS] per lo stile di esempio trovato in:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## Best practice di sviluppo {#development-best-practices}

### Best practice HTML {#html-best-practices}

* L’HTML (generato tramite HTL) deve essere strutturalmente semantico possibile; evitare inutili raggruppamenti/nidificazioni di elementi.
* Gli elementi HTML devono essere indirizzati tramite classi CSS in stile BEM.

**Buono** : tutti gli elementi del componente sono indirizzabili tramite la notazione BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Scorretto** : gli elementi elenco ed elenco sono indirizzabili solo in base al nome dell’elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* È meglio esporre più dati e nasconderli piuttosto che esporre troppi dati che richiedono uno sviluppo back-end futuro per esporli.

   * L’implementazione di pulsanti per contenuti modificabili dall’autore consente di mantenere l’HTML snello, consentendo agli autori di selezionare gli elementi di contenuto scritti nell’HTML. L’ può essere particolarmente importante quando si scrivono immagini nell’HTML che potrebbero non essere utilizzate per tutti gli stili.
   * L’eccezione a questa regola è quando le risorse costose, ad esempio le immagini, sono esposte per impostazione predefinita, in quanto le immagini dell’evento nascoste da CSS vengono, in questo caso, recuperate inutilmente.

      * I componenti immagine moderni spesso utilizzano JavaScript per selezionare e caricare l’immagine più appropriata per il caso d’uso (viewport).

### Best practice CSS {#css-best-practices}

>[!NOTE]
>
>Il sistema di stili crea una piccola divergenza tecnica rispetto a [BEM](https://en.bem.info/), in quanto le `BLOCK` e `BLOCK--MODIFIER` non vengono applicate allo stesso elemento, come specificato da [BEM](https://en.bem.info/).
>
>Al contrario, a causa di vincoli di prodotto, il valore `BLOCK--MODIFIER` viene applicato all&#39;elemento principale dell&#39;elemento `BLOCK`.
>
>Tutti gli altri tenant di [BEM](https://en.bem.info/) devono essere allineati con.

* Utilizza i preprocessori come [LESS](https://lesscss.org/) (supportato da AEM nativamente) o [SCSS](https://sass-lang.com/) (richiede un sistema di compilazione personalizzato) per consentire una chiara definizione CSS e la riutilizzabilità.

* mantenere uniforme il peso/specificità del selettore; Questo aiuta a evitare e risolvere i conflitti CSS difficili da identificare.
* Organizza ogni stile in un file discreto.
   * Questi file possono essere combinati utilizzando LESS/SCSS `@imports` o se è richiesto un CSS non elaborato, tramite l’inclusione di file nella libreria client HTML o sistemi di creazione di risorse front-end personalizzati.
* Evita di mescolare molti stili complessi.
   * Più stili possono essere applicati contemporaneamente a un componente, maggiore è la varietà di permutazioni. Questo può diventare difficile da mantenere/QA/garantire l’allineamento del marchio.
* Utilizza sempre le classi CSS (seguendo la notazione BEM) per definire regole CSS.
   * Se la selezione di elementi senza classi CSS (ovvero elementi vuoti) è assolutamente necessaria, spostali più in alto nella definizione CSS per chiarire che hanno una specificità inferiore rispetto a qualsiasi conflitto con elementi di quel tipo che hanno classi CSS selezionabili.
* Evita di applicare lo stile `BLOCK--MODIFIER` direttamente in quanto è collegato alla griglia reattiva. La modifica della visualizzazione di questo elemento può influenzare il rendering e la funzionalità della griglia reattiva, quindi solo lo stile a questo livello quando l’intento è quello di modificare il comportamento della griglia reattiva.
* Applica l’ambito dello stile utilizzando `BLOCK--MODIFIER`. Il `BLOCK__ELEMENT--MODIFIERS` può essere utilizzato nel componente, ma poiché il `BLOCK` rappresenta il componente e il componente è lo stile, lo stile viene &quot;definito&quot; e delimitato tramite `BLOCK--MODIFIER`.

Esempio di struttura del selettore CSS:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Selettore di primo livello</p> <p>BLOCCO - MODIFICATORE</p> </td> 
   <td valign="bottom"><p>Selettore di secondo livello</p> <p>BLOCCO</p> </td> 
   <td valign="bottom"><p>Selettore di terzo livello</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Selettore CSS efficace</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—scura</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—scura</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item {  </span></strong></p> <p><strong> colore: blu;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> colore: rosso;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Nel caso dei componenti nidificati, la profondità del selettore CSS per questi elementi dei componenti nidificati supererà il selettore di terzo livello. Ripetere lo stesso pattern per il componente nidificato, ma con l’ambito del componente principale `BLOCK`. In altre parole, avvia il `BLOCK` del componente nidificato al terzo livello e il `ELEMENT` del componente nidificato sarà al quarto livello di selettore.

### Best practice JavaScript {#javascript-best-practices}

Le best practice definite in questa sezione riguardano &quot;style-JavaScript&quot; o JavaScript appositamente progettato per manipolare il componente a scopo stilistico, anziché funzionale.

* Style-JavaScript deve essere utilizzato in modo giudizioso ed è un caso d’uso di minoranza.
* Style-JavaScript deve essere utilizzato principalmente per manipolare il DOM del componente per supportare lo stile CSS.
* Rivalutare l’utilizzo di Javascript se i componenti vengono visualizzati più volte su una pagina e comprendere il costo di elaborazione/ridisegno.
* Valuta nuovamente l’utilizzo di Javascript se richiama nuovi dati/contenuti in modo asincrono (tramite AJAX) quando il componente può essere visualizzato più volte su una pagina.
* Gestisci le esperienze di pubblicazione e authoring.
* Riutilizza style-Javascript quando possibile.
   * Ad esempio, se più stili di un componente richiedono che l’immagine del componente venga spostata in un’immagine di sfondo, lo stile JavaScript può essere implementato una volta e allegato a più `BLOCK--MODIFIERs`.
* Se possibile, separa lo stile-JavaScript da JavaScript funzionale.
* Valuta il costo di JavaScript rispetto alla manifestazione di queste modifiche DOM nell’HTML direttamente tramite HTL.
   * Quando un componente che utilizza lo stile JavaScript richiede una modifica lato server, valuta se la manipolazione JavaScript può essere introdotta in questo momento e quali sono gli effetti/ramificazioni sulle prestazioni e la supportabilità del componente.

#### Considerazioni sulle prestazioni {#performance-considerations}

* Style-JavaScript deve essere mantenuto leggero e snello.
* Per evitare sfarfallii e riutilizzi non necessari, nasconde inizialmente il componente tramite `BLOCK--MODIFIER BLOCK` e mostralo quando tutte le manipolazioni DOM in JavaScript sono completate.
* Le prestazioni delle manipolazioni JavaScript di stile sono simili ai plug-in jQuery di base che si attaccano e modificano gli elementi su DOMReady.
* Assicurati che le richieste siano compresse e che CSS e JavaScript siano ridotti.

## Risorse aggiuntive {#additional-resources}

* [Documentazione del sistema di stili](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creazione AEM librerie client](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sito web della documentazione di BEM (modificatore di elemento isolato)](https://getbem.com/)
* [Sito web della documentazione SNELESS](https://lesscss.org/)
* [Sito web jQuery](https://jquery.com/)
