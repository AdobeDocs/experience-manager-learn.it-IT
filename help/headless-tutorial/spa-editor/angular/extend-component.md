---
title: Estendere un componente | Guida introduttiva all’editor di SPA AEM e all’Angular
description: Scopri come estendere un componente core esistente da utilizzare con l’editor di SPA AEM. Scopri come aggiungere proprietà e contenuto a un componente esistente è una tecnica potente per espandere le funzionalità di un’implementazione di AEM Editor SPA. Scopri come utilizzare il pattern di delega per l’estensione dei modelli Sling e delle funzioni di Sling Resource Merger.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1935'
ht-degree: 2%

---

# Estendere un componente core {#extend-component}

Scopri come estendere un componente core esistente da utilizzare con l’editor di SPA AEM. Scopri come estendere un componente esistente è una tecnica potente per personalizzare ed espandere le funzionalità di un’implementazione di AEM Editor .

## Obiettivo

1. Estendi un componente core esistente con proprietà e contenuto aggiuntivi.
2. Comprendere la base dell’ereditarietà dei componenti con l’utilizzo di `sling:resourceSuperType`.
3. Scopri come utilizzare il [Pattern di delega](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) per i modelli Sling per riutilizzare la logica e le funzionalità esistenti.

## Cosa verrà creato

In questo capitolo, un nuovo `Card` viene creato. La `Card` estensione del componente [Componente core immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=it) aggiunta di campi di contenuto aggiuntivi come un titolo e un pulsante Invito all’azione per eseguire il ruolo di un teaser per altri contenuti all’interno dell’SPA.

![Authoring finale del componente per schede](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In un&#39;implementazione reale, può essere più appropriato utilizzare semplicemente il [Componente teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) che estendano [Componente core immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) per creare un `Card` a seconda dei requisiti del progetto. Si consiglia sempre di utilizzare [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) direttamente quando possibile.

## Prerequisiti

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi le `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installa il pacchetto finito per il tradizionale [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). Le immagini fornite da [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) viene riutilizzato nel SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) o controlla il codice localmente passando al ramo `Angular/extend-component-solution`.

## Implementazione iniziale della scheda Inspect

Il codice iniziale del capitolo fornisce un componente scheda iniziale. Inspect è il punto di partenza per l’implementazione di Card.

1. Nell’IDE che preferisci, apri le `ui.apps` modulo .
2. Passa a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` e visualizza `.content.xml` file.

   ![Inizio definizione componente a schede AEM](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   La proprietà `sling:resourceSuperType` punti `wknd-spa-angular/components/image` che indicano che `Card` eredita la funzionalità dal componente Immagine SPA WKND.

3. Inspect il file `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Tieni presente che `sling:resourceSuperType` punti `core/wcm/components/image/v2/image`. Questo indica che il componente Immagine SPA WKND eredita la funzionalità dall’immagine del componente core.

   Noto anche come [Pattern proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) L’ereditarietà di risorse Sling è un potente pattern di progettazione che consente ai componenti secondari di ereditare funzionalità ed estendere/sovrascrivere il comportamento quando desiderato. L’ereditarietà Sling supporta più livelli di ereditarietà, quindi in ultima analisi la nuova `Card` Il componente eredita la funzionalità dell’immagine del componente core.

   Molti team di sviluppo si sforzano di essere D.R.Y. (non ripetersi). L’ereditarietà Sling lo rende possibile con AEM.

4. Sotto `card` cartella, apri il file `_cq_dialog/.content.xml`.

   Questo file è la definizione della finestra di dialogo del componente per `Card` componente. Se utilizzi l’ereditarietà Sling, è possibile utilizzare le funzioni di [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=it) per ignorare o estendere parti della finestra di dialogo. In questo esempio, è stata aggiunta una nuova scheda alla finestra di dialogo per acquisire dati aggiuntivi da un autore per compilare il componente Scheda.

   Proprietà simili `sling:orderBefore` consentire agli sviluppatori di scegliere dove inserire nuove schede o campi modulo. In questo caso, il `Text` viene inserita prima della `asset` scheda . Per sfruttare appieno lo Sling Resource Merger, è importante conoscere la struttura del nodo di dialogo originale per il [Finestra di dialogo del componente immagine](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Sotto `card` cartella, apri il file `_cq_editConfig.xml`. Questo file determina il comportamento di trascinamento nell’interfaccia utente di AEM authoring. Quando estendi il componente Immagine, è importante che il tipo di risorsa corrisponda al componente stesso. Consulta la sezione `<parameters>` nodo:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La maggior parte dei componenti non richiede un `cq:editConfig`, i discendenti immagine e figlio del componente Immagine sono eccezioni.

6. Nello switch IDE al `ui.frontend` modulo, passaggio a `ui.frontend/src/app/components/card`:

   ![Avvio componente Angular](assets/extend-component/angular-card-component-start.png)

7. Inspect il file `card.component.ts`.

   Il componente è già stato bloccato per la mappatura sul AEM `Card` Componente che utilizza lo standard `MapTo` funzione .

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Rivedi i tre `@Input` nella classe per `src`, `alt`e `title`. Questi sono valori JSON attesi dal componente AEM mappato al componente Angular.

8. Aprire il file `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In questo esempio abbiamo scelto di riutilizzare il componente Immagine Angular esistente `app-image` semplicemente passando `@Input` parametri da `card.component.ts`. Successivamente nell’esercitazione, vengono aggiunte e visualizzate ulteriori proprietà.

## Aggiornare i criteri dei modelli

Con questa iniziale `Card` implementazione esamina la funzionalità nell’editor di SPA AEM. Per visualizzare il `Card` È necessario un aggiornamento del criterio del modello.

1. Distribuisci il codice iniziale in un&#39;istanza locale di AEM, se non lo hai già fatto:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Passa al modello di pagina SPA in [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aggiornare i criteri del Contenitore di layout per aggiungere il nuovo `Card` come componente consentito:

   ![Aggiorna i criteri dei contenitori di layout](assets/extend-component/card-component-allowed.png)

   Salva le modifiche al criterio e osserva il `Card` come componente consentito:

   ![Componente a schede come componente consentito](assets/extend-component/card-component-allowed-layout-container.png)

## Componente scheda iniziale autore

Quindi, crea le `Card` mediante l’editor SPA AEM.

1. Passa a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` aggiungi la `Card` nella `Layout Container`:

   ![Inserisci nuovo componente](assets/extend-component/insert-custom-component.png)

3. Trascina e rilascia un’immagine da Asset Finder nella `Card` componente:

   ![Aggiungi immagine](assets/extend-component/card-add-image.png)

4. Apri `Card` finestra di dialogo dei componenti e notare l’aggiunta di un **Testo** Tab.
5. Immetti i seguenti valori nel **Testo** scheda:

   ![Scheda Componente testo](assets/extend-component/card-component-text.png)

   **Percorso scheda** - scegli una pagina sotto la home page di SPA.

   **Testo CTA** - &quot;Ulteriori informazioni&quot;

   **Titolo della scheda** - lasciare vuoto

   **Ottieni titolo dalla pagina collegata** - seleziona la casella di controllo per indicare true.

6. Aggiorna **Metadati risorsa** scheda per aggiungere valori per **Testo alternativo** e **Didascalia**.

   Al momento non vengono visualizzate ulteriori modifiche dopo l’aggiornamento della finestra di dialogo. Per esporre i nuovi campi al componente Angular, è necessario aggiornare il modello Sling per `Card` componente.

7. Apri una nuova scheda e passa a [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect i nodi di contenuto sotto `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` per trovare `Card` contenuto componente.

   ![Proprietà dei componenti CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Osserva che le proprietà `cardPath`, `ctaText`, `titleFromPage` vengono mantenuti dalla finestra di dialogo.

## Aggiorna il modello Sling della scheda

Per esporre in ultima analisi i valori dalla finestra di dialogo del componente al componente Angular, è necessario aggiornare il modello Sling che popola il JSON per il `Card` componente. Abbiamo anche l&#39;opportunità di implementare due elementi di logica di business:

* Se `titleFromPage` a **true**, restituisce il titolo della pagina specificata da `cardPath` altrimenti restituisce il valore di `cardTitle` campo di testo.
* Restituisce la data dell’ultima modifica della pagina specificata da `cardPath`.

Torna all’IDE che preferisci e apri la `core` modulo .

1. Apri il file . `Card.java` a `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Osserva che il `Card` interfaccia attualmente estesa `com.adobe.cq.wcm.core.components.models.Image` e pertanto eredita i metodi `Image` interfaccia. La `Image` l&#39;interfaccia estende già `ComponentExporter` Interfaccia che consente l’esportazione del modello Sling come JSON e mappato dall’editor SPA. Pertanto non è necessario estendere esplicitamente `ComponentExporter` interfaccia come quella di [Capitolo Componente personalizzato](custom-component.md).

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

   Questi metodi vengono esposti tramite l’API del modello JSON e passati al componente Angular .

3. Apri `CardImpl.java`. Questa è l&#39;attuazione `Card.java` interfaccia. Questa implementazione è stata parzialmente bloccata per accelerare l’esercitazione.  Osserva l&#39;uso `@Model` e `@Exporter` annotazioni per garantire che il modello Sling possa essere serializzato come JSON tramite l’esportazione di modelli Sling.

   `CardImpl.java` utilizza anche [Pattern di delega per modelli Sling](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) per evitare di riscrivere la logica dal componente di base Immagine .

4. Osserva le seguenti righe:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L’annotazione sopra crea un’istanza di un oggetto Immagine denominato `image` in base ai `sling:resourceSuperType` eredità del `Card` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   È quindi possibile utilizzare semplicemente il `image` oggetto per implementare i metodi definiti dal `Image` interfaccia, senza dover scrivere noi stessi la logica. Questa tecnica viene utilizzata per `getSrc()`, `getAlt()`e `getTitle()`.

5. Quindi, implementa `initModel()` metodo per avviare una variabile privata `cardPage` in base al valore di `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   La `@PostConstruct initModel()` viene chiamato quando il modello Sling viene inizializzato, pertanto è una buona opportunità per inizializzare oggetti che possono essere utilizzati da altri metodi nel modello. La `pageManager` è uno dei diversi [Oggetti globali supportati da Java™](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) messi a disposizione di Sling Models tramite `@ScriptVariable` annotazione. La [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) prende un percorso e restituisce un AEM [Pagina](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) oggetto o nullo se il percorso non punta a una pagina valida.

   Inizializza il `cardPage` , che viene utilizzata dagli altri metodi per restituire dati sulla pagina collegata sottostante.

6. Esamina le variabili globali già mappate alle proprietà JCR salvate nella finestra di dialogo dell’autore. La `@ValueMapValue` viene utilizzata per eseguire automaticamente la mappatura.

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

   Queste variabili vengono utilizzate per implementare i metodi aggiuntivi per `Card.java` interfaccia.

7. Implementa i metodi aggiuntivi definiti nella `Card.java` interfaccia:

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
   > È possibile visualizzare [Ho finito CardImpl.java qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Apri una finestra terminale e distribuisci solo gli aggiornamenti della `core` modulo che utilizza Maven `autoInstallBundle` dal profilo `core` directory.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi le `classic` profilo.

9. Visualizza la risposta del modello JSON in: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e cerca `wknd-spa-angular/components/card`:

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

   Il modello JSON viene aggiornato con coppie chiave/valore aggiuntive dopo l’aggiornamento dei metodi in `CardImpl` Modello Sling.

## Aggiorna componente Angular

Ora che il modello JSON è popolato con nuove proprietà per `ctaLinkURL`, `ctaText`, `cardTitle`e `cardLastModified` possiamo aggiornare il componente Angular per visualizzarli.

1. Torna all’IDE e apri la `ui.frontend` modulo . Facoltativamente, avviare il server webpack dev da una nuova finestra terminale per visualizzare le modifiche in tempo reale:

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

3. Aggiungi i metodi per verificare se l’invito all’azione è pronto e per restituire una stringa di data/ora basata su `cardLastModified` input:

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

4. Apri `card.component.html` e aggiungi il seguente markup per visualizzare il titolo, l’invito all’azione e l’ultima data di modifica:

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

   Le regole Sass sono già state aggiunte in `card.component.scss` per assegnare uno stile al titolo, invoca l’azione e l’ultima data di modifica.

   >[!NOTE]
   >
   > È possibile visualizzare il completamento [Angular il codice del componente della scheda qui](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Distribuisci le modifiche complete da AEM dalla radice del progetto utilizzando Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Passa a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) per visualizzare il componente aggiornato:

   ![Componente scheda aggiornato in AEM](assets/extend-component/updated-card-in-aem.png)

7. Per creare una pagina simile alla seguente, dovresti essere in grado di ricreare il contenuto esistente:

   ![Authoring finale del componente per schede](assets/extend-component/final-authoring-card.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a estendere un componente AEM e come modelli e finestre di dialogo Sling funzionano con il modello JSON.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) o controlla il codice localmente passando al ramo `Angular/extend-component-solution`.
