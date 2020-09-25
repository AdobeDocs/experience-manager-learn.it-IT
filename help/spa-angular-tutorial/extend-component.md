---
title: Estendi un componente | Guida introduttiva all'editor SPA AEM e Angular
description: Scoprite come estendere un componente core esistente da utilizzare con AEM SPA Editor. La comprensione di come aggiungere proprietà e contenuto a un componente esistente è una tecnica potente per espandere le capacità di un’implementazione di AEM SPA Editor. Scopri come utilizzare il linguaggio di delega per estendere i modelli Sling e le funzionalità di Sling Resource Merger.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1984'
ht-degree: 2%

---


# Estendere un componente core {#extend-component}

Scoprite come estendere un componente core esistente da utilizzare con AEM SPA Editor. La comprensione di come estendere un componente esistente è una tecnica potente per personalizzare ed espandere le funzionalità di un’implementazione di Editor SPA AEM.

## Obiettivo

1. Estendi un componente core esistente con proprietà e contenuto aggiuntivi.
2. Comprendere le caratteristiche di base dell&#39;ereditarietà dei componenti con l&#39;uso di `sling:resourceSuperType`.
3. Scoprite come sfruttare il pattern [di](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) delega per i modelli Sling per riutilizzare la logica e le funzionalità esistenti.

## Cosa verrà creato

In questo capitolo viene creato un nuovo `Card` componente. Il `Card` componente espanderà il componente [](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/components/image.html) Immagine di base aggiungendo altri campi di contenuto, come un titolo e un pulsante Invito all’azione, per eseguire il ruolo di un teaser per altri contenuti nell’area SPA.

![Authoring finale del componente scheda](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In un’implementazione reale, potrebbe essere più appropriato utilizzare semplicemente il componente [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) Teaser, quindi estendere il componente [di base dell’](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/components/image.html) immagine per creare un `Card` componente in base ai requisiti del progetto. È sempre consigliabile utilizzare i componenti [](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html) core direttamente quando possibile.

## Prerequisiti

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

### Ottenere il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Distribuire la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzate [AEM 6.x](overview.md#compatibility) , aggiungete il `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installate il pacchetto finito per il sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest)WKND tradizionale. Le immagini fornite dal sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND verranno riutilizzate nell&#39;SPA WKND. Il pacchetto può essere installato tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) o estrarre il codice localmente passando al ramo `Angular/extend-component-solution`.

##  implementazione iniziale della scheda Inspect

Un componente scheda iniziale è stato fornito dal codice iniziale del capitolo.  Inspect il punto di partenza per l&#39;implementazione di Card.

1. Nell&#39;IDE di vostra scelta, aprite il `ui.apps` modulo.
2. Individuare `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` e visualizzare il `.content.xml` file.

   ![Inizio definizione AEM componente scheda](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   La proprietà `sling:resourceSuperType` indica `wknd-spa-angular/components/image` che il `Card` componente erediterà tutte le funzionalità dal componente Immagine SPA WKND.

3.  Inspect il file `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Notate che i `sling:resourceSuperType` punti a `core/wcm/components/image/v2/image`. Questo indica che il componente Immagine SPA WKND eredita tutte le funzionalità dall’Immagine componente principale.

   L&#39;ereditarietà delle risorse Sling del pattern [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Proxy è anche un potente pattern di progettazione che consente ai componenti secondari di ereditare le funzionalità e, se necessario, estendere/ignorare il comportamento. L’ereditarietà Sling supporta più livelli di ereditarietà, pertanto in ultima istanza il nuovo `Card` componente eredita la funzionalità dell’immagine del componente principale.

   Molti team di sviluppo si sforzano di essere D.R.Y. (non ripeterti). Sling eredità rende possibile questo con AEM.

4. Sotto la `card` cartella, aprite il file `_cq_dialog/.content.xml`.

   Questo file è la definizione della finestra di dialogo dei componenti per il `Card` componente. Se si utilizza l&#39;ereditarietà Sling, è possibile utilizzare le funzioni di Fusione [risorse](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) Sling per ignorare o estendere porzioni della finestra di dialogo. In questo esempio è stata aggiunta una nuova scheda alla finestra di dialogo per acquisire dati aggiuntivi da un autore e compilare il componente scheda.

   Proprietà come `sling:orderBefore` consentono agli sviluppatori di scegliere dove inserire nuove schede o campi modulo. In questo caso la `Text` scheda viene inserita prima della `asset` scheda. Per sfruttare appieno la fusione Sling Resource è importante conoscere la struttura del nodo di dialogo originale per la finestra di dialogo [del componente](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)Immagine.

5. Sotto la `card` cartella, aprite il file `_cq_editConfig.xml`. Questo file determina il comportamento di trascinamento nell’interfaccia di authoring AEM. Quando si estende il componente Immagine, è importante che il tipo di risorsa corrisponda al componente stesso. Rivedete il `<parameters>` nodo:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La maggior parte dei componenti non richiede un `cq:editConfig`, mentre i discendenti immagine e figlio del componente Immagine sono eccezioni.

6. Nel passaggio IDE al `ui.frontend` modulo, passare a `ui.frontend/src/app/components/card`:

   ![Inizio componente angolare](assets/extend-component/angular-card-component-start.png)

7.  Inspect il file `card.component.ts`.

   Il componente è già stato sovrapposto per la mappatura sul componente AEM `Card` utilizzando la `MapTo` funzione standard.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Esaminare i tre `@Input` parametri della classe per `src`, `alt`e `title`. Questi sono valori JSON previsti dal componente AEM che verrà mappato sul componente Angular.

8. Aprire il file `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In questo esempio abbiamo scelto di riutilizzare il componente Immagine angolare esistente `app-image` semplicemente trasmettendo i `@Input` parametri da `card.component.ts`. Più avanti nell&#39;esercitazione verranno aggiunte e visualizzate ulteriori proprietà.

## Aggiornare i criteri dei modelli

Con questa `Card` implementazione iniziale è possibile esaminare le funzionalità di AEM SPA Editor. Per visualizzare il `Card` componente iniziale è necessario aggiornare il criterio Modello.

1. Distribuisci il codice di avvio in un’istanza locale di AEM, se non hai già:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Andate al modello di pagina SPA all&#39;indirizzo [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aggiornate il criterio Contenitore di layout per aggiungere il nuovo `Card` componente come componente consentito:

   ![Aggiorna criterio Contenitore di layout](assets/extend-component/card-component-allowed.png)

   Salvare le modifiche al criterio e osservare il `Card` componente come componente consentito:

   ![Componente scheda come componente consentito](assets/extend-component/card-component-allowed-layout-container.png)

## Componente scheda iniziale autore

Quindi, create il `Card` componente utilizzando l&#39;Editor AEM SPA.

1. Andate a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` modalità, aggiungi il `Card` componente alla `Layout Container`:

   ![Inserisci nuovo componente](assets/extend-component/insert-custom-component.png)

3. Drag and drop an image from the Asset finder onto the `Card` component:

   ![Aggiungi immagine](assets/extend-component/card-add-image.png)

4. Aprire la finestra di dialogo del `Card` componente e osservare l’aggiunta di una scheda **Testo** .
5. Immettete i seguenti valori nella scheda **Testo** :

   ![Scheda Componente testo](assets/extend-component/card-component-text.png)

   **Percorso** scheda - scegliete una pagina sotto la pagina SPA.

   **Testo** CTA - &quot;Leggi tutto&quot;

   **Titolo** scheda - lasciate vuoto

   **Ottieni titolo dalla pagina** collegata: seleziona la casella di controllo per indicare true.

6. Aggiornate la scheda Metadati **** risorsa per aggiungere valori per Testo **** alternativo e **Didascalia**.

   Al momento non vengono visualizzate altre modifiche dopo l’aggiornamento della finestra di dialogo. Per esporre i nuovi campi al componente Angular, è necessario aggiornare il modello Sling per il `Card` componente.

7. Open a new tab and navigate to [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card).  i nodi di contenuto sottostanti `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` per individuare il contenuto del `Card` componente.

   ![Proprietà dei componenti CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Osservate che le proprietà `cardPath`, `ctaText`, `titleFromPage` sono persistenti nella finestra di dialogo.

## Aggiorna modello Sling scheda

Per esporre i valori dalla finestra di dialogo dei componenti al componente Angular, è necessario aggiornare il modello Sling che popola il JSON per il `Card` componente. Abbiamo anche l&#39;opportunità di implementare due elementi di business logic:

* Se `titleFromPage` il valore è **true**, restituire il titolo della pagina specificata in `cardPath` caso contrario restituirà il valore di `cardTitle` textfield.
* Restituisce la data dell’ultima modifica della pagina specificata da `cardPath`.

Tornate all&#39;IDE di vostra scelta e aprite il `core` modulo.

1. Open the file `Card.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Osservate che l&#39; `Card` interfaccia al momento si estende `com.adobe.cq.wcm.core.components.models.Image` e quindi eredita tutti i metodi dell&#39; `Image` interfaccia. L&#39; `Image` interfaccia estende già l&#39; `ComponentExporter` interfaccia che consente l&#39;esportazione del modello Sling come JSON e la mappatura da parte dell&#39;editor SPA. Pertanto non è necessario estendere esplicitamente `ComponentExporter` l&#39;interfaccia come abbiamo fatto nel capitolo [Componenti](custom-component.md)personalizzati.

2. Aggiungete i seguenti metodi all&#39;interfaccia:

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

   Questi metodi saranno esposti tramite l&#39;API del modello JSON e passati al componente Angular.

3. Apri `CardImpl.java`. Questa è l&#39;implementazione dell&#39; `Card.java` interfaccia. Questa implementazione è già stata parzialmente bloccata per accelerare l&#39;esercitazione.  Osservate l&#39;uso delle `@Model` annotazioni e delle `@Exporter` annotazioni per garantire che il modello Sling possa essere serializzato come JSON tramite Sling Model Exporter.

   `CardImpl.java` utilizza anche il pattern [Delega per i modelli](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Sling per evitare di riscrittura di tutta la logica dal componente di base Immagine.

4. Osservate le seguenti righe:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L’annotazione precedente crea un’istanza di un oggetto Immagine denominato `image` in base all’ `sling:resourceSuperType` ereditarietà del `Card` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   È quindi possibile utilizzare semplicemente l&#39; `image` oggetto per implementare i metodi definiti dall&#39; `Image` interfaccia, senza dover scrivere la logica da soli. Questa tecnica viene utilizzata per `getSrc()`, `getAlt()` e `getTitle()`.

5. Quindi, implementare il `initModel()` metodo per avviare una variabile privata `cardPage` in base al valore di `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   L&#39; `@PostConstruct initModel()` oggetto verrà sempre chiamato quando il modello Sling viene inizializzato, pertanto è una buona opportunità per inizializzare oggetti che possono essere utilizzati da altri metodi nel modello. L&#39; `pageManager` utente è uno dei numerosi oggetti [globali](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) Java supportati resi disponibili a Sling Models tramite l&#39; `@ScriptVariable` annotazione. Il metodo [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) occupa un percorso e restituisce un oggetto [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) AEM o un valore null se il percorso non punta a una pagina valida.

   In questo modo si inizializzerà la `cardPage` variabile, che verrà utilizzata dagli altri nuovi metodi per restituire i dati sulla pagina collegata sottostante.

6. Esaminate le variabili globali già mappate alle proprietà JCR salvate la finestra di dialogo di authoring. L&#39; `@ValueMapValue` annotazione viene utilizzata per eseguire automaticamente la mappatura.

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

   Queste variabili verranno utilizzate per implementare i metodi aggiuntivi per l&#39; `Card.java` interfaccia.

7. Implementare i metodi aggiuntivi definiti nell&#39; `Card.java` interfaccia:

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
   > È possibile visualizzare il CardImpl.java [completato qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Aprite una finestra terminale e distribuite solo gli aggiornamenti al `core` modulo utilizzando il profilo Maven `autoInstallBundle` dalla `core` directory.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Se utilizzate [AEM 6.x](overview.md#compatibility) , aggiungete il `classic` profilo.

9. Visualizzate la risposta del modello JSON all&#39;indirizzo: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e cercare `wknd-spa-angular/components/card`:

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

   Notate che il modello JSON viene aggiornato con coppie chiave/valore aggiuntive dopo l&#39;aggiornamento dei metodi nel modello `CardImpl` Sling.

## Aggiorna componente angolare

Ora che il modello JSON è popolato con nuove proprietà per `ctaLinkURL`, `ctaText`, `cardTitle` e `cardLastModified` possiamo aggiornare il componente Angular per visualizzarle.

1. Tornare all&#39;IDE e aprire il `ui.frontend` modulo. Facoltativamente, avviare il server di sviluppo webpack da una nuova finestra terminale per visualizzare le modifiche in tempo reale:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Apri `card.component.ts` a `ui.frontend/src/app/components/card/card.component.ts`. Aggiungete le `@Input` annotazioni aggiuntive per acquisire il nuovo modello:

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

3. Aggiungere metodi per verificare se l&#39;invito all&#39;azione è pronto e per restituire una stringa data/ora in base all&#39; `cardLastModified` input:

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

4. Apri `card.component.html` e aggiungi la seguente marcatura per visualizzare il titolo, la chiamata all’azione e l’ultima data di modifica:

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

   Sono già state aggiunte delle regole `card.component.scss` di base in corrispondenza dello stile del titolo, della chiamata all&#39;azione e dell&#39;ultima data di modifica.

   >[!NOTE]
   >
   > Puoi visualizzare il codice componente per scheda [angolare completato qui](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Distribuisci le modifiche complete a AEM dalla radice del progetto utilizzando Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Andate a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) per visualizzare il componente aggiornato:

   ![Componente scheda aggiornato in AEM](assets/extend-component/updated-card-in-aem.png)

7. Per creare una pagina simile alle seguenti, è necessario poter creare nuovamente il contenuto esistente:

   ![Authoring finale del componente scheda](assets/extend-component/final-authoring-card.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a estendere un componente AEM utilizzando i modelli Sling e come funzionano le finestre di dialogo con il modello JSON.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) o estrarre il codice localmente passando al ramo `Angular/extend-component-solution`.
