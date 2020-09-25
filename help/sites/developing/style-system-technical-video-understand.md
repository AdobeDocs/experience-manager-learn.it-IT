---
title: Come codificare il sistema di stile AEM
description: Questo video illustra l’anatomia del CSS (o LESS) e del codice JavaScript utilizzato per definire lo stile del componente Titolo principale di Adobe Experience Manager utilizzando il sistema di stile, nonché la modalità di applicazione di tali stili all’HTML e al DOM.
feature: style-system
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 664d3964df796d508973067f8fa4fe5ef83c5fec
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 1%

---


# Informazioni sul codice per il sistema di stile{#understanding-how-to-code-for-the-aem-style-system}

In questo video vedremo l&#39;anatomia dei CSS (o [!DNL LESS]) e JavaScript utilizzati per personalizzare il componente Titolo principale di Experience Manager con il sistema di stile, nonché come questi stili vengono applicati all&#39;HTML e al DOM.

>[!NOTE]
>
>Il AEM Style System è stato introdotto con [AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp1-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593).
>
>Il video presuppone che il componente Titolo We.Retail sia stato aggiornato per ereditare dai componenti [core v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases).

## Informazioni sul codice per il sistema di stile {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

Il pacchetto AEM fornito (**technical-review.sites.style-system-1.0.0.zip**) installa lo stile titolo di esempio, i criteri di esempio per i componenti Contenitore di layout e Titolo We.Retail e una pagina di esempio.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

Di seguito viene illustrata la [!DNL LESS] definizione dello stile di esempio trovato in:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Per coloro che preferiscono il CSS, sotto questo frammento di codice si trova il CSS in cui [!DNL LESS] viene compilato.

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

Quanto sopra [!DNL LESS] viene compilato in modo nativo  Experience Manager nel seguente CSS.

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

Il seguente codice JavaScript raccoglie e inserisce l&#39;ultima data e ora di modifica della pagina corrente sotto il testo del titolo quando lo stile di esempio viene applicato al componente Titolo.

L&#39;utilizzo di jQuery è facoltativo, così come le convenzioni di denominazione utilizzate.

Di seguito viene illustrata la [!DNL LESS] definizione dello stile di esempio trovato in:

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

* HTML (generato tramite HTL) dovrebbe essere il più strutturalmente possibile semantico; evitare inutili raggruppamenti/nidificazioni di elementi.
* Gli elementi HTML devono essere indirizzabili tramite classi CSS in stile BEM.

**Buona** : tutti gli elementi del componente possono essere indirizzati tramite la notazione BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Scorretto** - Gli elementi elenco e elenco possono essere indirizzati solo in base al nome dell&#39;elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* È meglio esporre più dati e nasconderli piuttosto che esporre troppo pochi dati che richiedono uno sviluppo back-end futuro per esporli.

   * L’implementazione di opzioni di attivazione/disattivazione per contenuti creativi può facilitare la creazione di questo codice HTML, consentendo agli autori di selezionare gli elementi di contenuto scritti nell’HTML. Questa funzione può essere particolarmente importante quando si scrivono immagini in formato HTML che potrebbero non essere utilizzate per tutti gli stili.
   * L&#39;eccezione a questa regola è rappresentata dal caso in cui le risorse costose, ad esempio le immagini, sono esposte per impostazione predefinita, in quanto le immagini dell&#39;evento nascoste da CSS vengono, in questo caso, recuperate inutilmente.

      * I componenti immagine moderni spesso utilizzano JavaScript per selezionare e caricare l’immagine più appropriata per l’uso (viewport).

### Best practice CSS {#css-best-practices}

>[!NOTE]
>
>Il sistema di stile crea una piccola divergenza tecnica da [BEM](https://en.bem.info/), in quanto `BLOCK` e non `BLOCK--MODIFIER` vengono applicati allo stesso elemento, come specificato da [BEM](https://en.bem.info/).
>
>Al contrario, a causa di vincoli del prodotto, il `BLOCK--MODIFIER` prodotto viene applicato all&#39;elemento padre dell&#39; `BLOCK` elemento.
>
>Tutti gli altri tenant di [BEM](https://en.bem.info/) devono essere allineati con.

* Utilizzate preprocessori quali [LESS](https://lesscss.org/) (supportati da AEM nativi) o [SCSS](https://sass-lang.com/) (richiede un sistema di compilazione personalizzato) per consentire una definizione CSS chiara e la riutilizzabilità.

* mantenere uniforme il peso/specificità del selettore; In questo modo si evitano e si risolvono conflitti CSS difficili da identificare.
* Organizzate ogni stile in un file separato.
   * Questi file possono essere combinati utilizzando LESS/SCSS `@imports` o se è richiesto CSS non elaborato, tramite l&#39;inclusione di file nella libreria client HTML o sistemi personalizzati di creazione di risorse front-end.
* Evitate di mescolare molti stili complessi.
   * Maggiore è il numero di stili che possono essere applicati contemporaneamente a un componente, maggiore è la varietà di mutazioni. Ciò può diventare difficile da mantenere/QA/garantire l&#39;allineamento del marchio.
* Utilizzate sempre le classi CSS (dopo la notazione BEM) per definire le regole CSS.
   * Se la selezione di elementi senza classi CSS (cioè elementi nudi) è assolutamente necessaria, spostateli più in alto nella definizione CSS per chiarire che hanno una specificità inferiore rispetto a qualsiasi conflitto con elementi di quel tipo che hanno classi CSS selezionabili.
* Evitate di applicare lo stile `BLOCK--MODIFIER` direttamente in quanto è collegato alla griglia reattiva. La modifica della visualizzazione di questo elemento può influenzare il rendering e la funzionalità della griglia reattiva, quindi solo lo stile a questo livello quando si intende modificare il comportamento della griglia reattiva.
* Applicare l&#39;ambito dello stile utilizzando `BLOCK--MODIFIER`. Il componente `BLOCK__ELEMENT--MODIFIERS` può essere utilizzato nel componente, ma poiché `BLOCK` rappresenta il componente e il componente è lo stile, lo stile è &quot;definito&quot; e l&#39;ambito è `BLOCK--MODIFIER`.

Esempio di struttura del selettore CSS:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Selettore di primo livello</p> <p>BLOCCO—MODIFICATORE</p> </td> 
   <td valign="bottom"><p>Selettore di secondo livello</p> <p>BLOCCO</p> </td> 
   <td valign="bottom"><p>Selettore di terzo livello</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Selettore CSS efficace</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> color: blu;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> color: rosso;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Nel caso di componenti nidificati, la profondità del selettore CSS per questi elementi dei componenti nidificati supererà il selettore di terzo livello. Ripetere lo stesso pattern per il componente nidificato, ma con l&#39;ambito del componente principale `BLOCK`. In altre parole, avviate il componente nidificato `BLOCK` al terzo livello, mentre il componente nidificato `ELEMENT` si trova al quarto livello di selettore.

### Best practice JavaScript {#javascript-best-practices}

Le best practice definite in questa sezione riguardano &quot;style-JavaScript&quot;, o JavaScript appositamente progettati per manipolare il componente a scopi stilistici, anziché funzionali.

* Style-JavaScript deve essere utilizzato in modo prudente ed è un caso d&#39;uso minoritario.
* Style-JavaScript deve essere utilizzato principalmente per manipolare il DOM del componente per supportare lo stile da parte di CSS.
* Rivedete l&#39;utilizzo di Javascript se i componenti vengono visualizzati più volte su una pagina e comprendete il costo di calcolo/ridisegno.
* Se estrae nuovi dati/contenuti in modo asincrono (tramite AJAX) e se il componente può essere visualizzato più volte su una pagina, rivalutate l’utilizzo di Javascript.
* Gestite le esperienze di pubblicazione e authoring.
* Riutilizzate style-Javascript quando possibile.
   * Ad esempio, se più stili di un componente richiedono che l&#39;immagine del componente venga spostata in un&#39;immagine di sfondo, lo stile JavaScript può essere implementato una volta e collegato a più `BLOCK--MODIFIERs`.
* Se possibile, separare stile-JavaScript da JavaScript funzionale.
* Valutare il costo di JavaScript rispetto alla manifestazione di tali modifiche DOM nell’HTML direttamente tramite HTL.
   * Quando un componente che utilizza lo stile-JavaScript richiede una modifica lato server, verificare se la manipolazione JavaScript può essere portata in questo momento e quali effetti/ramificazioni determinano le prestazioni e la supportabilità del componente.

#### Considerazioni sulle prestazioni {#performance-considerations}

* Style-JavaScript deve essere mantenuto leggero e snello.
* Per evitare sfarfallii e ridisegni non necessari, nascondete inizialmente il componente tramite `BLOCK--MODIFIER BLOCK`e visualizzatelo al termine di tutte le manipolazioni DOM in JavaScript.
* Le prestazioni delle manipolazioni JavaScript di stile sono simili ai plug-in jQuery di base che si collegano e modificano elementi su DOMReady.
* Assicurati che le richieste vengano compresse e che CSS e JavaScript vengano ridotti.

## Risorse aggiuntive {#additional-resources}

* [Documentazione del sistema di stile](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creazione AEM librerie client](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sito Web della documentazione BEM (Block Element Modifier)](https://getbem.com/)
* [Sito Web sulla documentazione LESS](https://lesscss.org/)
* [Sito Web jQuery](https://jquery.com/)
