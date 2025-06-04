---
title: Creare un blocco
description: Crea un blocco per un sito web Edge Delivery Services modificabile con l’editor universale.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 9698c17a-0ac8-426d-bccb-729b048cabd1
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1767'
ht-degree: 100%

---

# Creare un nuovo blocco

In questo capitolo viene descritto come creare un nuovo blocco teaser modificabile per un sito web Edge Delivery Services tramite l’editor universale.

![Nuovo blocco teaser](./assets//5-new-block/teaser-block.png)

Il blocco, denominato `teaser`, presenta i seguenti elementi:

- **Immagine**: immagine visivamente coinvolgente.
- **Contenuto testo**:
   - **Titolo**: titolo efficace per richiamare l’attenzione.
   - **Corpo del testo**: contenuto descrittivo che fornisce contesto o dettagli, ed eventuali termini e condizioni.
   - **Pulsante Call-to-Action (CTA)**: collegamento progettato per invitare l’utente a interagire e guidarlo verso un ulteriore coinvolgimento.

Il contenuto del blocco `teaser` può essere modificato nell’editor universale per garantirne la facilità d’uso e di riutilizzo in tutto il sito web.

Come puoi osservare, il blocco `teaser` è simile al blocco standard `hero`; pertanto il blocco `teaser` è inteso solo come semplice esempio per illustrare i concetti di sviluppo.

## Creare un nuovo ramo Git

Per mantenere un flusso di lavoro pulito e organizzato, crea un nuovo ramo per ogni attività di sviluppo specifica. Questo ti aiuterà a evitare problemi di implementazione in produzione di codice incompleto o non testato.

1. **Inizia dal ramo principale**: utilizza il codice di produzione più aggiornato, per contare su una solida base.
2. **Recupera modifiche remote**: recupera gli aggiornamenti più recenti da GitHub per assicurarti di disporre del codice più recente prima di iniziare lo sviluppo.
   - Esempio: dopo aver unito le modifiche dal ramo `wknd-styles` in `main`, recupera gli aggiornamenti più recenti.
3. **Crea un nuovo ramo**:

```bash
# ~/Code/aem-wknd-eds-ue

$ git fetch origin  
$ git checkout -b teaser origin/main  
```

Una volta creato il ramo `teaser`, puoi iniziare a sviluppare il blocco Teaser.

## Cartella del blocco

Crea una nuova cartella denominata `teaser` nella directory `blocks` del progetto. Questa cartella contiene i file JSON, CSS e JavaScript del blocco, organizzando tutti i file del blocco in un’unica posizione:

```
# ~/Code/aem-wknd-eds-ue

/blocks/teaser
```

Il nome della cartella del blocco funge da ID del blocco e viene utilizzato per farvi riferimento durante lo sviluppo.

## File JSON del blocco

Il file JSON del blocco ne definisce tre aspetti chiave:

- **Definizione**: registra il blocco come componente modificabile nell’editor universale, collegandolo a un modello di blocco e facoltativamente a un filtro.
- **Modello**: specifica i campi di authoring del blocco e il modo in cui vengono visualizzati come HTML semantico di Edge Delivery Services.
- **Filtro**: configura le regole di filtro per limitare i contenitori a cui è possibile aggiungere il blocco tramite l’editor universale. La maggior parte dei blocchi non sono contenitori, ma i loro ID vengono aggiunti ai filtri di altri blocchi contenitore.

Crea un nuovo file in `/blocks/teaser/_teaser.json` con la seguente struttura iniziale, nell’ordine esatto riportato di seguito. Se le chiavi non sono nell’ordine corretto, potrebbero non essere create correttamente.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome file dell’esempio di codice riportato di seguito."}

```json
{
    "definitions": [],
    "models": [],
    "filters": []
}
```

### Modello di blocco

Il modello di blocco è una parte critica della configurazione del blocco, in quanto definisce:

1. l’esperienza di authoring definendo i campi disponibili per la modifica;

   i ![campi per l’editor universale](./assets/5-new-block/fields-in-universal-editor.png);

2. come vengono riprodotti i valori dei campi nell’HTML di Edge Delivery Services.

Ai modelli viene assegnato un `id` che corrisponde alla [definizione del blocco](#block-definition) e include un array `fields` per specificare i campi modificabili.

Ogni campo nell’array `fields` ha un oggetto JSON che include le seguenti proprietà obbligatorie:

| Proprietà JSON | Descrizione |
|---------------|-----------------------------------------------------------------------------------------------------------------------|
| `component` | [Tipo di campo](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#component-types), ad esempio `text`, `reference` o `aem-content`. |
| `name` | Nome del campo, con mappatura alla proprietà JCR in cui il valore viene memorizzato in AEM. |
| `label` | Etichetta mostrata agli autori nell’editor universale. |

Per un elenco completo delle proprietà, incluse quelle facoltative, consulta la [documentazione dei campi dell’editor universale](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#fields).

#### Progettazione del blocco

![Blocco teaser](./assets/5-new-block/block-design.png)

Il blocco teaser include i seguenti elementi modificabili:

1. **Immagine**: rappresenta il contenuto visivo del teaser.
2. **Contenuto testo**: include il titolo, il corpo del testo e il pulsante di invito all’azione, e si trova in un rettangolo bianco.
   - Il **titolo** e il **corpo del testo** possono essere creati tramite lo stesso editor Rich Text.
   - L’**invito all’azione (CTA)** può essere creato con un campo `text` per l’**etichetta** e un campo `aem-content` per il **collegamento** associato al testo dell’etichetta.

La progettazione del blocco teaser è suddivisa in questi due componenti logici (contenuto immagine e testo), garantendo un’esperienza di authoring strutturata e intuitiva per gli utenti.

### Campi del blocco

Definisci i campi necessari per il blocco: immagine, testo alternativo per l’immagine, testo, etichetta CTA e collegamento CTA.

>[!BEGINTABS]

>[!TAB Cosa fare]

**Questa scheda illustra come modellare correttamente il blocco teaser.**

Il teaser è costituito da due aree logiche: immagine e testo. Per semplificare il codice necessario affinché l’HTML di Edge Delivery Services riproduca correttamente l’esperienza web desiderata, struttura il modello del blocco come segue.

- Raggruppa l’**immagine** e il **testo alternativo per l’immagine** utilizzando la [compressione dei campi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).
- Raggruppa i campi di contenuto di testo utilizzando il [raggruppamento di elementi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) e la [compressione dei campi per l’elemento CTA](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).

La [compressione dei campi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse), il [raggruppamento di elementi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) e l’[inferenza del tipo](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference) sono concetti essenziali per creare un modello di blocco ben strutturato; se non hai familiarità con questi concetti, consulta la documentazione mediante il relativo collegamento.

Nell’esempio seguente:

- L’[inferenza del tipo](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference) viene utilizzata per creare automaticamente un elemento HTML `<img>` dal campo `image`. La compressione dei campi viene utilizzata per i campi `image` e `imageAlt`, in modo da creare un elemento HTML `<img>`. L’attributo `src` è impostato con il valore del campo `image`, mentre l’attributo `alt` è impostato con il valore del campo `imageAlt`.
- `textContent` è un nome di gruppo utilizzato per categorizzare i campi. Deve essere semantico, ma può essere qualsiasi cosa specifica di questo blocco. Segnala all’editor universale di riprodurre, nell’output HTML finale, tutti i campi con questo prefisso all’interno dello stesso elemento `<div>` .
- La compressione del campo viene applicata anche nel gruppo `textContent` per l’elemento di invito all’azione (CTA). Il CTA viene creato come `<a>` tramite l’[inferenza del tipo](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference). Il campo `cta` viene utilizzato per impostare l’attributo `href` dell’elemento `<a>` e il campo `ctaText` fornisce il contenuto di testo per il collegamento all’interno dei tag `<a ...>`.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome file dell’esempio di codice riportato di seguito."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

Questo modello definisce, per il blocco, quali saranno gli input di authoring nell’editor universale.

Nell’HTML di Edge Delivery Services risultante per questo blocco, l’immagine inserita nel primo div e i campi del gruppo di elementi `textContent` nel secondo div.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image fields  -->
        <picture>
            ...
            <source .../>            
            <img src="..." alt="The authored alt text"/>
        </picture>
    </div>
    <div>
        <!-- This div, via element grouping contains the textContent fields -->
        <h2>The authored title</h2>
        <p>The authored body text</p>
        <a href="/authored/cta/link">The authored CTA label</a>
    </div>
</div>        
```

Come illustrato [nel prossimo capitolo](./7a-block-css.md), con questa struttura HTML sarà più semplice assegnare uno stile al blocco considerandolo una singola unità coesiva.

 La scheda **Cosa non fare** illustra cosa accade se non si comprimono i campi e non si raggruppano gli elementi.

>[!TAB Cosa non fare]

**Questa scheda descrive un modo non ottimale per modellare il blocco teaser, in giustapposizione al modo corretto descritto nella scheda adiacente.**

Si può essere tentati di definire ogni campo come elemento autonomo nel modello del blocco, senza ricorrere alla [compressione dei campi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) e al [raggruppamento di elementi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping). Tuttavia, questo complicherebbe l’assegnazione di uno stile al blocco considerandolo come una singola unità coesiva.

Ad esempio, il modello teaser potrebbe essere definito **senza** compressione dei campi o raggruppamento di elementi, come segue:

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome file dell’esempio di codice riportato di seguito."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "alt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "link",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "label",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

Nell’HTML di Edge Delivery Services per il blocco, il valore di ogni campo viene inserito in un elemento `div` separato e risulterà quindi più complicato comprendere il contenuto, applicarvi uno stile e regolare la struttura HTML per ottenere l’aspetto desiderato.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image  -->
        <picture>
            ...
            <source .../>            
            <img src="/authored/image/reference"/>
        </picture>
    </div>
    <div>
        <p>The authored alt text</p>
    </div>
    <div>
        <h2>The authored title</h2>
        <p>The authored body text</p>
    </div>
    <div>
        <a href="/authored/cta/link">/authored/cta/link</a>
    </div>
    <div>
        The authored CTA label
    </div>
</div>        
```

Ogni campo è isolato nel proprio `div`, e questo rende difficile formattare l’immagine e il contenuto di testo come unità coesive. Sarà comunque possibile ottenere l’aspetto finale desiderato, ma richiederà più impegno e creatività. Se invece si ricorre al [raggruppamento di elementi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) per raggruppare i campi di contenuto di testo e alla [compressione dei campi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) per aggiungere valori di authoring come attributi di elementi, risulterà tutto più facile, più semplice e semanticamente corretto.

Consulta la scheda **Cosa fare** qui sopra per informazioni su come modellare al meglio il blocco teaser.

>[!ENDTABS]


### Definizione del blocco

La definizione del blocco registra il blocco nell’editor universale. Di seguito trovi le proprietà JSON utilizzate nella definizione del blocco:

| Proprietà JSON | Descrizione |
|---------------|-------------|
| `definition.title` | Titolo del blocco così come viene visualizzato nella funzione **Aggiungi blocchi** dell’editor universale. |
| `definition.id` | ID univoco del blocco, utilizzato per controllarne l’utilizzo in `filters`. |
| `definition.plugins.xwalk.page.resourceType` | Definisce il tipo di risorsa Sling per il rendering del componente nell’editor universale. Utilizza sempre un tipo di risorsa `core/franklin/components/block/v#/block`. |
| `definition.plugins.xwalk.page.template.name` | Nome del blocco. Deve essere in caratteri minuscoli e con le parole unite da trattini, e corrispondere al nome della cartella del blocco. Questo valore viene utilizzato anche come etichetta dell’istanza del blocco nell’editor universale. |
| `definition.plugins.xwalk.page.template.model` | Collega questa definizione alla relativa definizione `model`, che controlla i campi di authoring che vengono visualizzati nell’editor universale per il blocco in questione. Il valore qui deve corrispondere a un valore `model.id`. |
| `definition.plugins.xwalk.page.template.classes` | Proprietà facoltativa, il cui valore viene aggiunto all’attributo `class` dell’elemento HTML del blocco. Questo consente varianti dello stesso blocco. Il valore `classes` può essere reso modificabile [aggiungendo un campo di classi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options) al [modello](#block-model) del blocco. |


Di seguito è riportato un esempio di codice JSON per la definizione del blocco:

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome file dell’esempio di codice riportato di seguito."}

```json
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

In questo esempio:

- Il blocco è denominato “Teaser” e utilizza il modello `teaser` che determina i campi che potranno essere modificati nell’editor universale.
- Il blocco include il contenuto predefinito del campo `textContent_text` (un’area Rich Text per il titolo e il corpo del testo), nonché `textContent_cta` e `textContent_ctaText` per il collegamento e l’etichetta dell’elemento CTA (invito all’azione). I nomi dei campi del modello contenenti il contenuto iniziale corrispondono ai nomi dei campi definiti nell’[array dei campi del modello di contenuto](#block-model);

Con questa struttura, nell’editor universale il blocco sarà configurato con i campi, il modello di contenuto e il tipo di risorsa giusti per consentirne la corretta riproduzione.

### Filtri del blocco

L’array `filters` del blocco definisce, per i [blocchi contenitore](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container), quali altri blocchi possono essere aggiunti al contenitore. I filtri definiscono un elenco di ID di blocco (`model.id`) che possono essere aggiunti al contenitore.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome file dell’esempio di codice riportato di seguito."}

```json
{
  "definitions": [... populated from previous section ...],
  "models": [... populated from previous section ...],
  "filters": []
}
```

Il componente teaser non è un [blocco contenitore](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container), pertanto non è possibile aggiungervi altri blocchi. Di conseguenza, l’array `filters` è lasciato vuoto. Piuttosto, aggiungi invece l’ID del teaser all’elenco dei filtri di un blocco sezione, in modo che possa essere aggiunto a una sezione.

![Filtri del blocco](./assets/5-new-block/filters.png)

I filtri dei blocchi forniti da Adobe, ad esempio il blocco sezione, sono memorizzati nella cartella `models` del progetto. Per modificarli, individua il file JSON del blocco fornito da Adobe (ad esempio, `/models/_section.json`) e aggiungi l’ID del teaser (`teaser`) all’elenco dei filtri. Questa configurazione segnala all’editor universale che il componente teaser può essere aggiunto al blocco contenitore “section”.

[!BADGE /models/_section.json]{type=Neutral tooltip="Nome file dell’esempio di codice riportato di seguito."}

```json
{
  "definitions": [],
  "models": [],
  "filters": [
    {
      "id": "section",
      "components": [
        "text",
        "image",
        "button",
        "title",
        "hero",
        "cards",
        "columns",
        "fragment",
        "teaser"
      ]
    }
  ]
}
```

L’ID di definizione del blocco teaser `teaser` è stato aggiunto all’array `components`.

## Eseguire il linting dei file JSON

Assicurati di [eseguire frequentemente il linting](./3-local-development-environment.md#linting) per le modifiche apportate, per assicurarti che il codice pulito e coerente. Eseguendo frequentemente il linting si possono individuare per tempo eventuali problemi, velocizzando così i tempi di sviluppo. Il comando `npm run lint:js` eseue il linting anche sui file JSON e rileva eventuali errori di sintassi.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:js
```

## Creare il progetto JSON

Dopo aver configurato i file JSON del blocco (ad esempio, `blocks/teaser/_teaser.json` e `models/_section.json`), questi vengono compilati automaticamente nei file `component-models.json`, `component-definitions.json` e `component-filters.json` del progetto. La compilazione viene gestita automaticamente da un hook pre-commit [Husky](https://typicode.github.io/husky/) incluso nel [modello del progetto AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk).

Le build possono anche essere attivate manualmente o a livello di programmazione tramite gli script NPM con codice [“build” di JSON](./3-local-development-environment.md#build-json-fragments) del progetto.

## Implementare il blocco JSON

Per rendere il blocco disponibile nell’editor universale, è necessario eseguire il commit del progetto e inviarlo al ramo di un archivio GitHub, in questo caso il ramo `teaser`.

Il nome esatto del ramo utilizzato dall’editor universale può essere regolato, per utente, tramite il suo URL.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add teaser block JSON files so it is available in Universal Editor"
# JSON files are compiled automatically and added to the commit via a husky precommit hook
$ git push origin teaser
```

Quando si apre l’editor universale con il parametro di query `?ref=teaser`, il nuovo blocco `teaser` viene visualizzato nella palette dei blocchi. Il blocco non ha uno stile; i campi del blocco vengono riprodotti come HTML semantico, e formattati solo tramite il [CSS globale](./4-website-branding.md#global-css).
