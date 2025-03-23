---
title: Come codificare per il sistema di stili di AEM
description: Questo video illustra l’anatomia del CSS (o LESS) e del JavaScript utilizzati per assegnare uno stile al componente Titolo core di Adobe Experience Manager utilizzando il sistema di stili, nonché il modo in cui questi stili vengono applicati al HTML e al DOM.
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1005
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# Informazioni su come codificare per il sistema di stili{#understanding-how-to-code-for-the-aem-style-system}

Questo video illustra l’anatomia del CSS (o [!DNL LESS]) e di JavaScript utilizzati per assegnare uno stile al componente Titolo core di Experience Manager utilizzando il sistema di stili, nonché il modo in cui questi stili vengono applicati al HTML e al DOM.


## Informazioni su come codificare per il sistema di stili {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

Il pacchetto AEM fornito (**technical-review.sites.style-system-1.0.0.zip**) installa lo stile del titolo dell&#39;esempio, i criteri di esempio per i componenti Contenitore di layout We.Retail e Titolo e una pagina di esempio.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

Di seguito è riportata la definizione [!DNL LESS] per lo stile di esempio trovato in:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Per chi preferisce CSS, sotto questo frammento di codice c&#39;è il CSS in cui [!DNL LESS] si compila.

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

L&#39;elemento [!DNL LESS] sopra riportato è stato compilato in modo nativo da Experience Manager nel seguente CSS.

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

Il seguente codice JavaScript raccoglie e inserisce la data e l’ora dell’ultima modifica apportata alla pagina corrente sotto il testo del titolo quando lo stile Esempio viene applicato al componente Titolo.

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

### Best practice per HTML {#html-best-practices}

* HTML (generato tramite HTL) deve essere strutturalmente il più semantico possibile; evitando inutili raggruppamenti o nidificazioni di elementi.
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

**Non valido** - L&#39;elenco e gli elementi dell&#39;elenco sono indirizzabili solo in base al nome dell&#39;elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* È meglio esporre più dati e nasconderli piuttosto che esporre troppi pochi dati che richiedono un futuro sviluppo back-end per esporli.

   * L’implementazione di contenuti accessibili all’autore può contribuire a mantenere snella questa HTML, consentendo agli autori di selezionare gli elementi di contenuto da scrivere in HTML. L’ può essere particolarmente importante quando si scrivono immagini sul HTML che potrebbero non essere utilizzate per tutti gli stili.
   * L’eccezione a questa regola si verifica quando risorse costose, come le immagini, vengono esposte per impostazione predefinita, in quanto le immagini di eventi nascoste dai CSS vengono recuperate inutilmente.

      * I componenti immagine moderni spesso utilizzano JavaScript per selezionare e caricare l’immagine più appropriata per il caso d’uso (riquadro di visualizzazione).

### Best practice CSS {#css-best-practices}

>[!NOTE]
>
>Il sistema di stili crea una piccola divergenza tecnica rispetto a [BEM](https://en.bem.info/), in quanto `BLOCK` e `BLOCK--MODIFIER` non sono applicati allo stesso elemento, come specificato da [BEM](https://en.bem.info/).
>
>A causa di vincoli di prodotto, invece, `BLOCK--MODIFIER` viene applicato all&#39;elemento padre dell&#39;elemento `BLOCK`.
>
>Tutti gli altri tenant di [BEM](https://en.bem.info/) devono essere allineati con.

* Utilizza i preprocessori come [LESS](https://lesscss.org/) (supportati in modo nativo da AEM) o [SCSS](https://sass-lang.com/) (richiede un sistema di compilazione personalizzato) per consentire una chiara definizione CSS e la riutilizzabilità.

* Mantieni uniforme il peso/specificità del selettore; questo consente di evitare e risolvere conflitti a catena CSS difficili da identificare.
* Organizzare ogni stile in un file discreto.
   * Questi file possono essere combinati utilizzando LESS/SCSS `@imports` o, se è richiesto un file CSS non elaborato, tramite l&#39;inclusione di file della libreria client di HTML o sistemi di creazione di risorse front-end personalizzati.
* Evita di mescolare molti stili complessi.
   * Più stili possono essere applicati a un componente in una sola volta, maggiore è la varietà di permutazioni. Può diventare difficile mantenere, verificare e garantire l&#39;allineamento del marchio.
* Utilizza sempre le classi CSS (seguendo la notazione BEM) per definire le regole CSS.
   * Se è assolutamente necessario selezionare elementi senza classi CSS (ovvero elementi vuoti), spostali più in alto nella definizione CSS per chiarire che hanno una specificità inferiore rispetto a qualsiasi conflitto con elementi di quel tipo che hanno classi CSS selezionabili.
* Evita di applicare direttamente lo stile a `BLOCK--MODIFIER`, poiché è collegato alla griglia reattiva. La modifica della visualizzazione di questo elemento può influire sul rendering e sulle funzionalità della griglia reattiva, quindi solo a questo livello lo stile ha l’obiettivo di modificare il comportamento della griglia reattiva.
* Applica ambito stile utilizzando `BLOCK--MODIFIER`. `BLOCK__ELEMENT--MODIFIERS` può essere utilizzato nel componente, ma poiché `BLOCK` rappresenta il componente e il componente è lo stile, lo stile è &quot;definito&quot; e con ambito `BLOCK--MODIFIER`.

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

Nel caso di componenti nidificati, la profondità del selettore CSS per questi elementi Componente nidificati supererà il selettore di terzo livello. Ripeti lo stesso pattern per il componente nidificato, ma con ambito `BLOCK` del componente principale. In altre parole, avviare `BLOCK` del componente nidificato al terzo livello e `ELEMENT` del componente nidificato al quarto livello del selettore.

### Best practice per JavaScript {#javascript-best-practices}

Le best practice definite in questa sezione si riferiscono a &quot;style-JavaScript&quot;, o JavaScript specificamente concepito per manipolare il componente a scopo stilistico, anziché funzionale.

* Style-JavaScript deve essere utilizzato con cautela ed è un caso di utilizzo minoritario.
* Style-JavaScript deve essere utilizzato principalmente per manipolare il DOM del componente in modo da supportare lo stile CSS.
* Valuta nuovamente l’utilizzo di JavaScript se i componenti verranno visualizzati più volte su una pagina e comprendi i costi di calcolo/ridisegno.
* Valuta nuovamente l’utilizzo di JavaScript se estrae nuovi dati/contenuti in modo asincrono (tramite AJAX) quando il componente può apparire più volte su una pagina.
* Gestisci sia le esperienze di pubblicazione che di authoring.
* Se possibile, riutilizza style-Javascript.
   * Se, ad esempio, più stili di un componente richiedono che l&#39;immagine di quest&#39;ultimo venga spostata in un&#39;immagine di sfondo, il componente style-JavaScript può essere implementato una sola volta e collegato a più `BLOCK--MODIFIERs`.
* Se possibile, separa style-JavaScript dal JavaScript funzionale.
* Valuta il costo di JavaScript rispetto alla manifestazione di queste modifiche DOM in HTML direttamente tramite HTL.
   * Quando un componente che utilizza style-JavaScript richiede una modifica lato server, valuta se è possibile inserire la manipolazione JavaScript in questo momento e quali sono gli effetti/ramificazioni delle prestazioni e della supportabilità del componente.

#### Considerazioni sulle prestazioni {#performance-considerations}

* Style-JavaScript deve essere tenuto leggero e snello.
* Per evitare sfarfallii e disegni inutili, nascondere inizialmente il componente tramite `BLOCK--MODIFIER BLOCK` e visualizzarlo al termine di tutte le manipolazioni DOM in JavaScript.
* Le prestazioni delle manipolazioni style-JavaScript sono simili ai plug-in jQuery di base che si collegano e modificano gli elementi in DOMReady.
* Assicurati che le richieste siano compresse e che CSS e JavaScript siano ridotti al minimo.

## Risorse aggiuntive {#additional-resources}

* [Documentazione del sistema di stili](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creazione di librerie client di AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/clientlibs.html)
* Sito Web della documentazione di [BEM (Block Element Modifier)](https://getbem.com/)
* [Sito Web della documentazione di minore entità](https://lesscss.org/)
* [jQuery sito Web](https://jquery.com/)
