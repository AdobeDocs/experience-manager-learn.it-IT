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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1961'
ht-degree: 1%

---

# Estendere un componente core {#extend-component}

Scopri come estendere un componente core esistente da utilizzare con l’editor di SPA AEM. Scopri come estendere un componente esistente è una tecnica potente per personalizzare ed espandere le funzionalità di un’implementazione di AEM Editor .

## Obiettivo

1. Estendi un componente core esistente con proprietà e contenuto aggiuntivi.
2. Comprendere le nozioni di base dell’ereditarietà dei componenti con l’utilizzo di `sling:resourceSuperType`.
3. Scopri come sfruttare il [Pattern di delega](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) per i modelli Sling per riutilizzare la logica e le funzionalità esistenti.

## Cosa verrà creato

In questo capitolo verrà creato un nuovo componente `Card` . Il componente `Card` estenderà il [componente di base immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) aggiungendo ulteriori campi di contenuto, come un titolo e un pulsante Invito all’azione, per eseguire il ruolo di un teaser per altri contenuti all’interno del SPA.

![Authoring finale del componente per schede](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In un’implementazione reale potrebbe essere più appropriato utilizzare semplicemente il [componente teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) e quindi estendere il [componente core immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) per creare un componente `Card` in base ai requisiti del progetto. Si consiglia sempre di utilizzare [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) direttamente quando possibile.

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

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

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installa il pacchetto finito per il tradizionale [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite dal [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) verranno riutilizzate nel SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) o estrarre il codice localmente passando al ramo `Angular/extend-component-solution`.

## Implementazione iniziale della scheda Inspect

Il codice iniziale del capitolo fornisce un componente scheda iniziale. Inspect è il punto di partenza per l’implementazione di Card.

1. Nell’IDE che preferisci, apri il modulo `ui.apps` .
2. Passa a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` e visualizza il file `.content.xml`.

   ![Inizio definizione componente a schede AEM](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   La proprietà `sling:resourceSuperType` punta a `wknd-spa-angular/components/image` per indicare che il componente `Card` erediterà tutte le funzionalità dal componente Immagine SPA WKND.

3. Inspect il file `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Tieni presente che il `sling:resourceSuperType` punta a `core/wcm/components/image/v2/image`. Questo indica che il componente Immagine SPA WKND eredita tutte le funzionalità dall’immagine del componente core.

   Anche noto come [Pattern proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) ereditarietà di risorse Sling è un potente pattern di progettazione per consentire ai componenti secondari di ereditare le funzionalità ed estendere/sovrascrivere il comportamento quando desiderato. L’ereditarietà Sling supporta più livelli di ereditarietà, pertanto in ultima analisi il nuovo componente `Card` eredita la funzionalità dell’immagine del componente core.

   Molti team di sviluppo si sforzano di essere D.R.Y. (non ripetersi). L’ereditarietà Sling lo rende possibile con AEM.

4. Sotto la cartella `card` , apri il file `_cq_dialog/.content.xml`.

   Questo file è la definizione della finestra di dialogo del componente `Card` . Se si utilizza l&#39;ereditarietà Sling, è possibile utilizzare le funzioni di [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) per sovrascrivere o estendere parti della finestra di dialogo. In questo esempio è stata aggiunta una nuova scheda alla finestra di dialogo per acquisire dati aggiuntivi da un autore per compilare il componente scheda.

   Proprietà come `sling:orderBefore` consentono agli sviluppatori di scegliere dove inserire nuove schede o campi modulo. In questo caso, la scheda `Text` verrà inserita prima della scheda `asset` . Per sfruttare appieno lo Sling Resource Merger è importante conoscere la struttura del nodo di dialogo originale per la finestra di dialogo [Componente immagine](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Sotto la cartella `card` , apri il file `_cq_editConfig.xml`. Questo file determina il comportamento di trascinamento nell’interfaccia utente di AEM authoring. Quando estendi il componente Immagine, è importante che il tipo di risorsa corrisponda al componente stesso. Rivedi il nodo `<parameters>`:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La maggior parte dei componenti non richiede un `cq:editConfig`, i discendenti immagine e figlio del componente Immagine sono eccezioni.

6. Nello switch IDE al modulo `ui.frontend`, passa a `ui.frontend/src/app/components/card`:

   ![Avvio componente Angular](assets/extend-component/angular-card-component-start.png)

7. Inspect il file `card.component.ts`.

   Il componente è già stato arrestato per essere mappato sul componente AEM `Card` utilizzando la funzione standard `MapTo`.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Esamina i tre parametri `@Input` nella classe per `src`, `alt` e `title`. Si tratta dei valori JSON previsti dal componente AEM che verrà mappato al componente Angular.

8. Aprire il file `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In questo esempio abbiamo scelto di riutilizzare il componente Immagine Angular esistente `app-image` semplicemente passando i parametri `@Input` da `card.component.ts`. Successivamente nell’esercitazione verranno aggiunte e visualizzate altre proprietà.

## Aggiornare i criteri dei modelli

Con questa implementazione iniziale `Card` , controlla la funzionalità nell’editor di SPA AEM. Per visualizzare il componente iniziale `Card` è necessario un aggiornamento al criterio del modello.

1. Distribuisci il codice iniziale in un&#39;istanza locale di AEM, se non lo hai già fatto:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Passa al SPA modello di pagina in [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aggiorna i criteri del Contenitore di layout per aggiungere il nuovo componente `Card` come componente consentito:

   ![Aggiorna i criteri dei contenitori di layout](assets/extend-component/card-component-allowed.png)

   Salva le modifiche al criterio e osserva il componente `Card` come componente consentito:

   ![Componente a schede come componente consentito](assets/extend-component/card-component-allowed-layout-container.png)

## Componente scheda iniziale autore

Quindi, crea il componente `Card` utilizzando l’editor di SPA AEM.

1. Passa a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In modalità `Edit` , aggiungi il componente `Card` al `Layout Container`:

   ![Inserisci nuovo componente](assets/extend-component/insert-custom-component.png)

3. Trascina un’immagine da Asset Finder al componente `Card` :

   ![Aggiungi immagine](assets/extend-component/card-add-image.png)

4. Apri la finestra di dialogo del componente `Card` e osserva l’aggiunta di una scheda **Testo** .
5. Immetti i seguenti valori nella scheda **Testo** :

   ![Scheda Componente testo](assets/extend-component/card-component-text.png)

   **Percorso scheda** : scegli una pagina sotto la home page di SPA.

   **Testo**  CTA - &quot;Ulteriori informazioni&quot;

   **Titolo**  della scheda - Lascia vuoto

   **Ottieni titolo dalla pagina**  collegata: seleziona la casella di controllo per indicare true.

6. Aggiorna la scheda **Metadati risorsa** per aggiungere valori per **Testo alternativo** e **Didascalia**.

   Al momento non vengono visualizzate ulteriori modifiche dopo l’aggiornamento della finestra di dialogo. Per esporre i nuovi campi al componente Angular, è necessario aggiornare il modello Sling per il componente `Card` .

7. Apri una nuova scheda e passa a [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect mostra i nodi di contenuto sotto `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` per trovare il contenuto del componente `Card`.

   ![Proprietà dei componenti CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Osserva che le proprietà `cardPath`, `ctaText`, `titleFromPage` vengono mantenute dalla finestra di dialogo.

## Aggiorna il modello Sling della scheda

Per esporre i valori dalla finestra di dialogo del componente al componente Angular, è necessario aggiornare il modello Sling che compila il JSON per il componente `Card`. Abbiamo anche l&#39;opportunità di implementare due elementi di logica di business:

* Se `titleFromPage` restituisce **true**, restituisce il titolo della pagina specificata da `cardPath` altrimenti restituisce il valore di `cardTitle` textfield.
* Restituisce l’ultima data di modifica della pagina specificata da `cardPath`.

Torna all’IDE che preferisci e apri il modulo `core` .

1. Apri il file `Card.java` in `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Osserva che l’interfaccia `Card` al momento si estende `com.adobe.cq.wcm.core.components.models.Image` e quindi eredita tutti i metodi dell’interfaccia `Image`. L’interfaccia `Image` estende già l’ interfaccia `ComponentExporter` che consente di esportare il modello Sling come JSON e di mapparlo dall’editor SPA. Pertanto non è necessario estendere esplicitamente l&#39;interfaccia `ComponentExporter` come abbiamo fatto nel capitolo [Componente personalizzato](custom-component.md).

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

   Questi metodi verranno esposti tramite l’API del modello JSON e passati al componente Angular .

3. Apri `CardImpl.java`. Questa è l’implementazione dell’interfaccia `Card.java` . Questa implementazione è già stata parzialmente bloccata per accelerare l’esercitazione.  Osserva l’uso delle annotazioni `@Model` e `@Exporter` per garantire che il modello Sling possa essere serializzato come JSON tramite l’esportatore di modelli Sling.

   `CardImpl.java` utilizza anche il pattern  [Delega per Sling ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Models per evitare di riscrivere tutta la logica dal componente di base Immagine .

4. Osserva le seguenti righe:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L’annotazione precedente crea un’istanza di un oggetto Immagine denominato `image` in base all’ `sling:resourceSuperType` ereditarietà del componente `Card` .

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   È quindi possibile utilizzare semplicemente l&#39;oggetto `image` per implementare i metodi definiti dall&#39;interfaccia `Image` senza dover scrivere personalmente la logica. Questa tecnica viene utilizzata per `getSrc()`, `getAlt()` e `getTitle()`.

5. Quindi, implementa il metodo `initModel()` per avviare una variabile privata `cardPage` in base al valore di `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   L’ `@PostConstruct initModel()` viene sempre chiamato quando si inizializza il modello Sling, pertanto è una buona opportunità per inizializzare gli oggetti che possono essere utilizzati da altri metodi nel modello. Il `pageManager` è uno dei numerosi [oggetti globali Java supportati](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) resi disponibili ai modelli Sling tramite l&#39;annotazione `@ScriptVariable` . Il metodo [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service-javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) accetta un percorso e restituisce un oggetto AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service-javadoc/com/day/cq/wcm/api/Page.html) o null se il percorso non punta a una pagina valida.

   Viene inizializzata la variabile `cardPage`, che verrà utilizzata dagli altri nuovi metodi per restituire i dati sulla pagina collegata sottostante.

6. Esamina le variabili globali già mappate alle proprietà JCR salvate nella finestra di dialogo dell’autore. L’annotazione `@ValueMapValue` viene utilizzata per eseguire automaticamente la mappatura.

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

   Queste variabili verranno utilizzate per implementare i metodi aggiuntivi per l&#39;interfaccia `Card.java` .

7. Implementa i metodi aggiuntivi definiti nell&#39;interfaccia `Card.java`:

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
   > Puoi visualizzare il [CardImpl.java finito qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Apri una finestra terminale e distribuisci solo gli aggiornamenti al modulo `core` utilizzando il profilo Maven `autoInstallBundle` dalla directory `core`.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi il profilo `classic`.

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

   Nota che il modello JSON viene aggiornato con coppie chiave/valore aggiuntive dopo l’aggiornamento dei metodi nel modello `CardImpl` Sling .

## Aggiorna componente Angular

Ora che il modello JSON è popolato con nuove proprietà per `ctaLinkURL`, `ctaText`, `cardTitle` e `cardLastModified`, possiamo aggiornare il componente Angular per visualizzarli.

1. Torna all’IDE e apri il modulo `ui.frontend` . Facoltativamente, avviare il server di sviluppo webpack da una nuova finestra terminale per visualizzare le modifiche in tempo reale:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Apri `card.component.ts` in `ui.frontend/src/app/components/card/card.component.ts`. Aggiungi le annotazioni `@Input` aggiuntive per acquisire il nuovo modello:

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

3. Aggiungi i metodi per verificare se l’invito all’azione è pronto e per restituire una stringa di data/ora basata sull’input `cardLastModified` :

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

4. Apri `card.component.html` e aggiungi il seguente markup per visualizzare il titolo, la chiamata all’azione e l’ultima data di modifica:

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

   Le regole di base sono già state aggiunte in `card.component.scss` per assegnare uno stile al titolo, alla chiamata all’azione e all’ultima data di modifica.

   >[!NOTE]
   >
   > Puoi visualizzare il codice del componente della scheda di Angular [completato qui](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Distribuisci le modifiche complete da AEM dalla radice del progetto utilizzando Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Passa a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) per visualizzare il componente aggiornato:

   ![Componente scheda aggiornato in AEM](assets/extend-component/updated-card-in-aem.png)

7. È necessario poter creare nuovamente il contenuto esistente per creare una pagina simile alla seguente:

   ![Authoring finale del componente per schede](assets/extend-component/final-authoring-card.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a estendere un componente AEM utilizzando e come modelli e finestre di dialogo Sling funzionano con il modello JSON.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) o estrarre il codice localmente passando al ramo `Angular/extend-component-solution`.
