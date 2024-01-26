---
title: Come programmare per il sistema di stili dell’AEM
description: Questo video illustra l’anatomia dei CSS (o LESS) e JavaScript utilizzati per assegnare uno stile al componente Titolo core di Adobe Experience Manager utilizzando il sistema di stili, nonché il modo in cui questi stili vengono applicati a HTML e DOM.
feature: Style System
version: 6.4, 6.5, Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1061
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# Informazioni su come codificare per il sistema di stili{#understanding-how-to-code-for-the-aem-style-system}

In questo video vedremo l’anatomia del CSS (o [!DNL LESS]) e JavaScript utilizzati per assegnare uno stile al componente Titolo principale di Experience Manager utilizzando il sistema di stili, nonché per come questi stili vengono applicati a HTML e DOM.


## Informazioni su come codificare per il sistema di stili {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

Il pacchetto AEM fornito (**technical-review.sites.style-system-1.0.0.zip**) installa lo stile del titolo dell&#39;esempio, i criteri di esempio per i componenti Contenitore di layout We.Retail e Titolo e una pagina di esempio.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

Di seguito è riportato [!DNL LESS] definizione dello stile di esempio disponibile in:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Per coloro che preferiscono CSS, sotto questo frammento di codice è il CSS questo [!DNL LESS] si compila in.

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

Quanto sopra [!DNL LESS] viene compilato in modo nativo da Experience Manager nel seguente CSS.

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

Il seguente codice JavaScript raccoglie e inserisce l’ultima data e ora modificata della pagina corrente sotto il testo del titolo quando lo stile Esempio viene applicato al componente Titolo.

L’utilizzo di jQuery è facoltativo, così come le convenzioni di denominazione utilizzate.

Di seguito è riportato [!DNL LESS] definizione dello stile di esempio disponibile in:

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

* HTML (generato tramite HTL) deve essere il più strutturalmente semantico possibile; evitando inutili raggruppamenti/nidificazioni di elementi.
* Gli elementi HTML devono essere indirizzabili tramite classi CSS in stile BEM.

**Buono** - Tutti gli elementi nel componente sono indirizzabili tramite la notazione BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Non valido** - L’elenco e gli elementi dell’elenco sono indirizzabili solo per nome elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* È meglio esporre più dati e nasconderli piuttosto che esporre troppi pochi dati che richiedono un futuro sviluppo back-end per esporli.

   * L’implementazione di opzioni per contenuto utilizzabile dall’autore può aiutare a mantenere questo HTML snello, consentendo agli autori di selezionare gli elementi di contenuto da scrivere nel HTML. L’ può essere particolarmente importante quando si scrivono immagini sul HTML che potrebbero non essere utilizzate per tutti gli stili.
   * L’eccezione a questa regola si verifica quando risorse costose, come le immagini, vengono esposte per impostazione predefinita, in quanto le immagini di eventi nascoste dai CSS vengono recuperate inutilmente.

      * I componenti immagine moderni spesso utilizzano JavaScript per selezionare e caricare l’immagine più appropriata per il caso d’uso (viewport).

### Best practice CSS {#css-best-practices}

>[!NOTE]
>
>Il sistema di stili crea una piccola divergenza tecnica [BEM](https://en.bem.info/), in quanto `BLOCK` e `BLOCK--MODIFIER` non vengono applicati allo stesso elemento, come specificato da [BEM](https://en.bem.info/).
>
>A causa di vincoli di prodotto, il `BLOCK--MODIFIER` viene applicato all&#39;elemento padre del `BLOCK` elemento.
>
>Tutti gli altri tenant di [BEM](https://en.bem.info/) deve essere allineato con.

* Utilizzare i preprocessori come [MENO](https://lesscss.org/) (con il supporto nativo dell&#39;AEM) o [SCSS](https://sass-lang.com/) (richiede un sistema di build personalizzato) per consentire una chiara definizione CSS e la riutilizzabilità.

* Mantieni uniforme il peso/specificità del selettore; questo consente di evitare e risolvere conflitti a catena CSS difficili da identificare.
* Organizzare ogni stile in un file discreto.
   * Questi file possono essere combinati utilizzando LESS/SCSS `@imports` o se è richiesto un CSS non elaborato, tramite l’inclusione di file della Libreria client di HTML o sistemi di creazione di risorse front-end personalizzati.
* Evita di mescolare molti stili complessi.
   * Più stili possono essere applicati a un componente in una sola volta, maggiore è la varietà di permutazioni. Può diventare difficile mantenere, verificare e garantire l&#39;allineamento del marchio.
* Utilizza sempre le classi CSS (seguendo la notazione BEM) per definire le regole CSS.
   * Se è assolutamente necessario selezionare elementi senza classi CSS (ovvero elementi vuoti), spostali più in alto nella definizione CSS per chiarire che hanno una specificità inferiore rispetto a qualsiasi conflitto con elementi di quel tipo che hanno classi CSS selezionabili.
* Evita di applicare lo stile a `BLOCK--MODIFIER` direttamente quando è collegato alla griglia reattiva. La modifica della visualizzazione di questo elemento può influire sul rendering e sulle funzionalità della griglia reattiva, quindi solo a questo livello lo stile ha l’obiettivo di modificare il comportamento della griglia reattiva.
* Applica ambito stile tramite `BLOCK--MODIFIER`. Il `BLOCK__ELEMENT--MODIFIERS` può essere utilizzato nel componente, ma poiché `BLOCK` rappresenta il Componente, il componente è formattato, lo stile è &quot;definito&quot; e con ambito tramite `BLOCK--MODIFIER`.

Esempio di struttura del selettore CSS:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Selettore di primo livello</p> <p>BLOCCA - MODIFICATORE</p> </td> 
   <td valign="bottom"><p>Selettore di secondo livello</p> <p>BLOCCA</p> </td> 
   <td valign="bottom"><p>Selettore di terzo livello</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Selettore CSS effettivo</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—scuro</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—scuro</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> colore: blu;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> colore: rosso;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Nel caso di componenti nidificati, la profondità del selettore CSS per questi elementi Componente nidificati supererà il selettore di terzo livello. Ripeti lo stesso pattern per il componente nidificato, ma con ambito dal componente principale `BLOCK`. In altre parole, avvia il `BLOCK` al terzo livello, e del componente nidificato `ELEMENT` si trova al quarto livello del selettore.

### Best practice per JavaScript {#javascript-best-practices}

Le best practice definite in questa sezione si riferiscono a &quot;style-JavaScript&quot;, o JavaScript specificamente progettato per manipolare il componente a scopo stilistico, anziché funzionale.

* Style-JavaScript deve essere utilizzato in modo giudizioso ed è un caso di utilizzo minoritario.
* Style-JavaScript deve essere utilizzato principalmente per manipolare il DOM del componente in modo da supportare lo stile CSS.
* Valuta nuovamente l’utilizzo di JavaScript se i componenti verranno visualizzati più volte su una pagina e comprendi i costi di calcolo/ridisegno.
* Valuta nuovamente l’utilizzo di JavaScript se estrae nuovi dati/contenuti in modo asincrono (tramite AJAX) quando il componente può apparire più volte su una pagina.
* Gestisci sia le esperienze di pubblicazione che di authoring.
* Se possibile, riutilizza style-Javascript.
   * Ad esempio, se più stili di un componente richiedono che l’immagine di quest’ultimo venga spostata su un’immagine di sfondo, il codice JavaScript per lo stile può essere implementato una volta e collegato a più `BLOCK--MODIFIERs`.
* Se possibile, separa style-JavaScript da JavaScript funzionale.
* Valuta il costo di JavaScript rispetto a manifestare queste modifiche DOM nel HTML direttamente tramite HTL.
   * Quando un componente che utilizza style-JavaScript richiede una modifica lato server, valuta se la manipolazione JavaScript può essere effettuata in questo momento e quali sono gli effetti/ramificazioni per le prestazioni e la supportabilità del componente.

#### Considerazioni sulle prestazioni {#performance-considerations}

* Style-JavaScript deve essere tenuto leggero e snello.
* Per evitare sfarfallii e richiami non necessari, nascondi inizialmente il componente tramite `BLOCK--MODIFIER BLOCK`e quando tutte le manipolazioni DOM nel JavaScript sono state completate.
* Le prestazioni delle manipolazioni style-JavaScript sono simili ai plug-in jQuery di base che si collegano e modificano gli elementi in DOMReady.
* Assicurati che le richieste siano compresse e che CSS e JavaScript siano ridotti al minimo.

## Risorse aggiuntive {#additional-resources}

* [Documentazione del sistema di stili](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creazione di librerie client AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sito Web della documentazione di BEM (Block Element Modifier)](https://getbem.com/)
* [Sito Web di documentazione LESS](https://lesscss.org/)
* [sito web jQuery](https://jquery.com/)
