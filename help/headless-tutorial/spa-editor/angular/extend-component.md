---
title: Estendere un componente | Guida introduttiva dell'Angular e dell'editor SPA dell'AEM
description: Scopri come estendere un Componente core esistente da utilizzare con l’Editor SPA dell’AEM. Comprendere come aggiungere proprietà e contenuti a un componente esistente è una tecnica efficace per espandere le funzionalità di un’implementazione dell’Editor SPA dell’AEM. Scopri come utilizzare il pattern di delega per estendere i modelli Sling e le funzioni di Sling Resource Merger.
feature: SPA Editor, Core Components
version: Cloud Service
jira: KT-5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
duration: 621
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1713'
ht-degree: 0%

---

# Estendere un componente core {#extend-component}

Scopri come estendere un Componente core esistente da utilizzare con l’Editor SPA dell’AEM. Scopri come estendere un componente esistente è una tecnica potente per personalizzare ed espandere le funzionalità di un’implementazione dell’Editor SPA dell’AEM.

## Obiettivo

1. Estendi un componente core esistente con proprietà e contenuti aggiuntivi.
2. Comprendere le nozioni di base dell’ereditarietà dei componenti con l’utilizzo di `sling:resourceSuperType`.
3. Scopri come utilizzare il [Pattern di delega](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) affinché i modelli Sling riutilizzino la logica e le funzionalità esistenti.

## Cosa verrà creato

In questo capitolo, viene visualizzata una nuova `Card` viene creato il componente. Il `Card` il componente estende [Componente core immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=it) l’aggiunta di ulteriori campi di contenuto, come un titolo e un pulsante di invito all’azione, per svolgere il ruolo di teaser per altri contenuti all’interno dell’SPA.

![Authoring finale del componente della scheda](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In un&#39;implementazione reale, potrebbe essere più appropriato utilizzare semplicemente il [Componente Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) che l&#39;estensione [Componente core immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=it) per creare un `Card` componente in base ai requisiti del progetto. Si consiglia sempre di utilizzare [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) direttamente quando possibile.

## Prerequisiti

Esaminare gli strumenti e le istruzioni necessari per l&#39;impostazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Distribuisci la base di codice in un’istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se si utilizza [AEM 6.x](overview.md#compatibility) aggiungi `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installare il pacchetto finito per il tradizionale [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). Le immagini fornite da [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) viene riutilizzato sul WKND SPA. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Gestione pacchetti installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) oppure estrarre il codice localmente passando al ramo `Angular/extend-component-solution`.

## Implementazione della carta iniziale di Inspect

Un componente iniziale della scheda è stato fornito dal codice iniziale del capitolo. Inspect è il punto di partenza per l’implementazione della scheda.

1. Nell’IDE che preferisci, apri `ui.apps` modulo.
2. Accedi a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` e visualizzare `.content.xml` file.

   ![Componente scheda AEM definizione Start](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   La proprietà `sling:resourceSuperType` punta a `wknd-spa-angular/components/image` che indica che `Card` Questo componente eredita la funzionalità dal componente WKND SPA Image.

3. Inspect il file `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Tieni presente che `sling:resourceSuperType` punta a `core/wcm/components/image/v2/image`. Questo indica che il componente Immagine SPA WKND eredita la funzionalità dall’immagine del componente core.

   Noto anche come [Modello proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) L’ereditarietà delle risorse Sling è un potente modello di progettazione che consente ai componenti secondari di ereditare funzionalità ed estendere/ignorare il comportamento quando desiderato. L’ereditarietà Sling supporta più livelli di ereditarietà, quindi in ultima analisi il nuovo `Card` Il componente eredita la funzionalità dell’immagine del componente core.

   Molti team di sviluppo si sforzano di essere D.R.Y. (non ripeterti). L’ereditarietà Sling lo rende possibile con l’AEM.

4. Sotto `card` cartella, apri il file `_cq_dialog/.content.xml`.

   Questo file è la definizione della finestra di dialogo del componente `Card` componente. Se utilizzi l’ereditarietà Sling, è possibile utilizzare le funzioni di [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=it) per sostituire o estendere parti della finestra di dialogo. In questo esempio, è stata aggiunta una nuova scheda alla finestra di dialogo per acquisire dati aggiuntivi da un autore per popolare il componente Scheda.

   Proprietà come `sling:orderBefore` consente a uno sviluppatore di scegliere dove inserire nuove schede o campi modulo. In questo caso, il `Text` viene inserita prima della scheda `asset` scheda. Per utilizzare appieno Sling Resource Merger, è importante conoscere la struttura originale dei nodi di dialogo per [Finestra di dialogo del componente immagine](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Sotto `card` cartella, apri il file `_cq_editConfig.xml`. Questo file determina il comportamento di trascinamento nell’interfaccia utente di creazione dell’AEM. Quando si estende il componente Immagine, è importante che il tipo di risorsa corrisponda al componente stesso. Rivedi `<parameters>` nodo:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La maggior parte dei componenti non richiede un `cq:editConfig`, Immagine e i discendenti secondari del componente Immagine sono eccezioni.

6. Nell&#39;IDE passare alla `ui.frontend` modulo, passare a `ui.frontend/src/app/components/card`:

   ![Inizio componente Angular](assets/extend-component/angular-card-component-start.png)

7. Inspect il file `card.component.ts`.

   Il componente è già stato sottoposto a stub out per la mappatura sull’AEM `Card` Componente che utilizza lo standard `MapTo` funzione.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Rivedi i tre `@Input` parametri nella classe per `src`, `alt`, e `title`. Si tratta di valori JSON previsti dal componente AEM mappati al componente Angular.

8. Apri il file `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In questo esempio abbiamo scelto di riutilizzare il componente immagine Angular esistente `app-image` passando semplicemente il `@Input` parametri da `card.component.ts`. Più avanti nell’esercitazione, vengono aggiunte e visualizzate ulteriori proprietà.

## Aggiornare il criterio del modello

Con questa iniziale `Card` rivedere la funzionalità nell’editor SPA dell’AEM. Per visualizzare il `Card` componente è necessario aggiornare il criterio Modello.

1. Distribuisci il codice di avvio in un’istanza locale dell’AEM, se non lo hai già fatto:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Passa al modello di pagina dell’SPA all’indirizzo [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aggiorna il criterio del Contenitore di layout per aggiungere il nuovo `Card` componente come componente consentito:

   ![Criterio Aggiorna contenitore layout](assets/extend-component/card-component-allowed.png)

   Salva le modifiche apportate al criterio e osserva `Card` componente come componente consentito:

   ![Componente scheda come componente consentito](assets/extend-component/card-component-allowed-layout-container.png)

## Componente carta iniziale autore

Quindi, crea il `Card` componente che utilizza l’editor SPA dell’AEM.

1. Accedi a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In entrata `Edit` , aggiungi il `Card` componente per `Layout Container`:

   ![Inserisci nuovo componente](assets/extend-component/insert-custom-component.png)

3. Trascina e rilascia un’immagine da Asset Finder su `Card` componente:

   ![Aggiungi immagine](assets/extend-component/card-add-image.png)

4. Apri `Card` e osserva l’aggiunta di un’ **Testo** Tab.
5. Immetti i seguenti valori su **Testo** scheda:

   ![Scheda Componente testo](assets/extend-component/card-component-text.png)

   **Percorso scheda** - scegli una pagina sotto la home page dell&#39;SPA.

   **Testo CTA** - &quot;Ulteriori informazioni&quot;

   **Titolo carta** - lascia vuoto

   **Ottieni titolo da pagina collegata** - seleziona la casella di controllo per indicare true.

6. Aggiornare il **Metadati risorsa** scheda per aggiungere valori per **Testo alternativo** e **Didascalia**.

   Al momento non vengono visualizzate ulteriori modifiche dopo l’aggiornamento della finestra di dialogo. Per esporre i nuovi campi al componente Angular, è necessario aggiornare il modello Sling per `Card` componente.

7. Apri una nuova scheda e passa a [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect i nodi di contenuto sotto `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` per trovare `Card` contenuto del componente.

   ![Proprietà del componente CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Osserva le proprietà `cardPath`, `ctaText`, `titleFromPage` vengono mantenuti dalla finestra di dialogo.

## Aggiorna modello Sling della scheda

Per esporre in definitiva i valori della finestra di dialogo del componente al componente Angular, è necessario aggiornare il modello Sling che compila il JSON per il `Card` componente. Abbiamo anche l’opportunità di implementare due parti della logica di business:

* Se `titleFromPage` a **true**, restituisce il titolo della pagina specificata da `cardPath` in caso contrario restituisce il valore di `cardTitle` campo di testo.
* Restituisce la data dell&#39;ultima modifica della pagina specificata da `cardPath`.

Torna all’IDE che preferisci e apri la `core` modulo.

1. Apri il file `Card.java` a `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Osserva che il `Card` l&#39;interfaccia attualmente estende `com.adobe.cq.wcm.core.components.models.Image` e pertanto eredita i metodi del `Image` di rete. Il `Image` L&#39;interfaccia di estende già `ComponentExporter` che consente di esportare il modello Sling come JSON e mapparlo tramite l’editor SPA. Pertanto non è necessario estendere esplicitamente `ComponentExporter` interfaccia come in [Capitolo componente personalizzato](custom-component.md).

2. Aggiungi i seguenti metodi all’interfaccia:

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   Questi metodi vengono esposti tramite l’API del modello JSON e passati al componente Angular.

3. Apri `CardImpl.java`. Questa è l&#39;implementazione di `Card.java` di rete. Questa implementazione è stata parzialmente testata per accelerare l’esercitazione.  Osserva l’utilizzo di `@Model` e `@Exporter` annotazioni per garantire che il modello Sling possa essere serializzato come JSON tramite Sling Model Exporter.

   `CardImpl.java` utilizza anche [Pattern di delega per modelli Sling](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) per evitare di riscrivere la logica dal componente core Immagine.

4. Osserva le righe seguenti:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L’annotazione precedente crea un’istanza di un oggetto Image denominato `image` in base al `sling:resourceSuperType` ereditarietà del `Card` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   È quindi possibile utilizzare semplicemente il `image` oggetto per implementare i metodi definiti dal `Image` senza dover scrivere direttamente la logica. Questa tecnica viene utilizzata per `getSrc()`, `getAlt()`, e `getTitle()`.

5. Quindi, implementa `initModel()` metodo per avviare una variabile privata `cardPage` in base al valore di `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Il `@PostConstruct initModel()` viene chiamato quando il modello Sling viene inizializzato, pertanto si tratta di una buona opportunità per inizializzare oggetti che possono essere utilizzati da altri metodi nel modello. Il `pageManager` è uno dei numerosi [Oggetti globali supportati da Java™](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) resi disponibili ai modelli Sling tramite `@ScriptVariable` annotazione. Il [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) Il metodo immette un percorso e restituisce un AEM [Pagina](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) object o null se il percorso non punta a una pagina valida.

   Inizializza il `cardPage` variabile, utilizzata dagli altri nuovi metodi per restituire dati sulla pagina collegata sottostante.

6. Rivedi le variabili globali già mappate alle proprietà JCR salvate nella finestra di dialogo di authoring. Il `@ValueMapValue` l’annotazione viene utilizzata per eseguire automaticamente la mappatura.

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   Queste variabili vengono utilizzate per implementare i metodi aggiuntivi per `Card.java` di rete.

7. Implementare i metodi aggiuntivi definiti nella `Card.java` Interfaccia:

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > È possibile visualizzare [ha completato CardImpl.java qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Apri una finestra del terminale e distribuisci solo gli aggiornamenti a `core` modulo di utilizzando Maven `autoInstallBundle` profilo da `core` directory.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Se si utilizza [AEM 6.x](overview.md#compatibility) aggiungi `classic` profilo.

9. Visualizza la risposta del modello JSON in: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e cerca il `wknd-spa-angular/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   Il modello JSON viene aggiornato con altre coppie chiave/valore dopo l’aggiornamento dei metodi in `CardImpl` Modello Sling.

## Aggiorna componente Angular

Ora che il modello JSON è compilato con nuove proprietà per `ctaLinkURL`, `ctaText`, `cardTitle`, e `cardLastModified` è possibile aggiornare il componente Angular per visualizzarli.

1. Torna all’IDE e apri la `ui.frontend` modulo. Se necessario, avviare il server di sviluppo Webpack da una nuova finestra del terminale per visualizzare le modifiche in tempo reale:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Apri `card.component.ts` a `ui.frontend/src/app/components/card/card.component.ts`. Aggiungi il `@Input` annotazioni per acquisire il nuovo modello:

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. Aggiungere metodi per verificare se l&#39;invito all&#39;azione è pronto e per restituire una stringa di data/ora basata sul `cardLastModified` input:

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. Apri `card.component.html` e aggiungi il seguente markup per visualizzare il titolo, l’invito all’azione e la data dell’ultima modifica:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   Le regole Sass sono già state aggiunte in `card.component.scss` per assegnare uno stile al titolo, all’azione e alla data dell’ultima modifica.

   >[!NOTE]
   >
   > È possibile visualizzare il [Angular di codice del componente della scheda qui](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Implementa le modifiche complete all’AEM dalla directory principale del progetto utilizzando Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Accedi a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) per visualizzare il componente aggiornato:

   ![Componente scheda aggiornato nell’AEM](assets/extend-component/updated-card-in-aem.png)

7. Dovresti essere in grado di riscrivere il contenuto esistente per creare una pagina simile alla seguente:

   ![Authoring finale del componente della scheda](assets/extend-component/final-authoring-card.png)

## Congratulazioni. {#congratulations}

Congratulazioni, hai imparato a estendere un componente AEM e come i modelli e le finestre di dialogo Sling funzionano con il modello JSON.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) oppure estrarre il codice localmente passando al ramo `Angular/extend-component-solution`.
