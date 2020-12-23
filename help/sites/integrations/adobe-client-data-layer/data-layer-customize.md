---
title: Personalizzare il livello dati client del Adobe  con AEM componenti
description: Scoprite come personalizzare il livello dati client  Adobe con il contenuto dei componenti AEM personalizzati. Scopri come utilizzare le API fornite da AEM Componenti di base per estendere e personalizzare il livello di dati.
feature: core-component
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
translation-type: tm+mt
source-git-commit: 46936876de355de9923f7a755aa6915a13cca354
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Personalizzare il livello dati client del Adobe  con AEM componenti {#customize-data-layer}

Scoprite come personalizzare il livello dati client  Adobe con il contenuto dei componenti AEM personalizzati. Scopri come utilizzare le API fornite da [AEM componenti core per estendere](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) e personalizzare il livello dati.

## Cosa verrà creato

![Livello dati breadline](assets/adobe-client-data-layer/byline-data-layer-html.png)

In questa esercitazione verranno illustrate diverse opzioni per estendere il livello dati client del Adobe  aggiornando il componente WKND [Byline](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html). Si tratta di un componente personalizzato e le lezioni apprese in questa esercitazione possono essere applicate ad altri componenti personalizzati.

### Obiettivi {#objective}

1. Iniettare i dati dei componenti nel livello dati estendendo un modello Sling e un componente HTL
1. Utilizzare le utility del livello dati dei componenti core per ridurre il carico di lavoro
1. Utilizzare gli attributi dei dati dei componenti core per collegarsi agli eventi dei livelli di dati esistenti

## Prerequisiti {#prerequisites}

Per completare l&#39;esercitazione è necessario un ambiente di sviluppo locale ****. Gli screenshot e i video vengono acquisiti utilizzando il AEM come SDK di Cloud Service in esecuzione su un macOS. I comandi e il codice sono indipendenti dal sistema operativo locale, se non diversamente specificato.

**Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

**Per prima cosa AEM 6.5?** Per configurare un ambiente [ di sviluppo locale, consulta la ](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)seguente guida.

## Scaricare e distribuire il sito di riferimento WKND {#set-up-wknd-site}

Questa esercitazione estende il componente Byline nel sito di riferimento WKND. Clona e installa la base di codice WKND nel tuo ambiente locale.

1. Avviate un&#39;istanza locale di Quickstart **author** AEM in esecuzione in [http://localhost:4502](Http://localhost:4502).
1. Aprite una finestra terminale e clonate la base di codice WKND utilizzando Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Distribuire la base di codice WKND in un’istanza locale di AEM:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se si utilizza AEM 6.5 e l&#39;ultimo service pack, aggiungere il profilo `classic` al comando Maven:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Apri una nuova finestra del browser e accedi a AEM. Aprite una pagina **Magazine** come: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Componente autore sulla pagina](assets/adobe-client-data-layer/byline-component-onpage.png)

   Dovresti vedere un esempio del componente Autore aggiunto alla pagina come parte di un frammento esperienza. Puoi visualizzare il frammento esperienza all&#39;indirizzo [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Aprite gli strumenti per sviluppatori e immettete il comando seguente nella **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

    Inspect la risposta per visualizzare lo stato corrente del livello dati su un sito AEM. Vengono visualizzate informazioni sulla pagina e sui singoli componenti.

   ![ risposta livello dati Adobe](assets/data-layer-state-response.png)

   Osservare che il componente Autore non è elencato nel Livello dati.

## Aggiornare il modello Sling autore {#sling-model}

Per inserire dati sul componente nel livello dati, è necessario aggiornare prima il modello Sling del componente. Quindi, aggiornate l&#39;interfaccia Java di Byline e l&#39;implementazione del modello Sling per aggiungere un nuovo metodo `getData()`. Questo metodo conterrà le proprietà che si desidera inserire nel livello dati.

1. Nell&#39;IDE di tua scelta, apri il progetto `aem-guides-wknd`. Andate al modulo `core`.
1. Aprire il file `Byline.java` in `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interfaccia Java Byline](assets/adobe-client-data-layer/byline-java-interface.png)

1. Aggiungere un nuovo metodo all’interfaccia:

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. Aprire il file `BylineImpl.java` in `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   Questa è l&#39;implementazione dell&#39;interfaccia `Byline` e viene implementata come modello Sling.

1. Aggiungete le seguenti istruzioni di importazione all&#39;inizio del file:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Le API `fasterxml.jackson` verranno utilizzate per serializzare i dati che si desidera esporre come JSON. Il `ComponentUtils` di AEM componenti core verrà utilizzato per verificare se il livello dati è abilitato.

1. Aggiungere il metodo non implementato `getData()` a `BylineImple.java`:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   Nel metodo precedente viene utilizzato un nuovo `HashMap` per acquisire le proprietà che si desidera esporre come JSON. Tenere presente che vengono utilizzati metodi esistenti come `getName()` e `getOccupations()`. `@type` rappresenta il tipo univoco di risorsa del componente, che consente al client di identificare facilmente eventi e/o attivatori in base al tipo di componente.

   `ObjectMapper` viene utilizzato per serializzare le proprietà e restituire una stringa JSON. Questa stringa JSON può quindi essere inserita nel livello dati.

1. Aprite una finestra terminale. Crea e distribuisci solo il modulo `core` utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Aggiornare il codice autore HTL {#htl}

Quindi, aggiornare `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL (HTML Template Language) è il modello utilizzato per il rendering del codice HTML del componente.

Per esporre il proprio livello di dati viene utilizzato un attributo di dati speciale `data-cmp-data-layer` su ciascun componente AEM.  JavaScript fornito da AEM Core Components cerca questo attributo di dati, il cui valore verrà popolato con la stringa JSON restituita dal metodo `getData()` del modello Sling autore, e inserisce i valori nel livello Dati client del Adobe .

1. Nell&#39;IDE aprire il progetto `aem-guides-wknd`. Andate al modulo `ui.apps`.
1. Aprire il file `byline.html` in `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Codice HTML autore](assets/adobe-client-data-layer/byline-html-template.png)

1. Aggiornate `byline.html` per includere l&#39;attributo `data-cmp-data-layer`:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   Il valore di `data-cmp-data-layer` è stato impostato su `"${byline.data}"` dove `byline` è il modello Sling aggiornato in precedenza. `.data` è la notazione standard per la chiamata di un metodo Java Getter in HTL di  `getData()` implementato nell&#39;esercizio precedente.

1. Aprite una finestra terminale. Crea e distribuisci solo il modulo `ui.apps` utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Tornate nel browser e riaprite la pagina con un componente Autore: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Aprite gli strumenti di sviluppo ed esaminate l’origine HTML della pagina per il componente Byline:

   ![Livello dati breadline](assets/adobe-client-data-layer/byline-data-layer-html.png)

   È opportuno verificare che la `data-cmp-data-layer` sia stata compilata con la stringa JSON dal modello Sling.

1. Aprite gli strumenti di sviluppo del browser e immettete il comando seguente nella **console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Andate sotto la risposta in `component` per trovare l&#39;istanza del componente `byline` è stata aggiunta al livello dati:

   ![Parte autore del livello dati](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Dovresti visualizzare una voce come segue:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Osservate che le proprietà esposte sono le stesse aggiunte in `HashMap` nel modello Sling.

## Aggiunta di un evento Click {#click-event}

Il livello dati client del Adobe  è basato sull&#39;evento e uno degli eventi più comuni per attivare un&#39;azione è l&#39;evento `cmp:click`. I componenti core AEM semplificano la registrazione del componente con l’aiuto dell’elemento dati: `data-cmp-clickable`.

Gli elementi selezionabili sono in genere un pulsante CTA o un collegamento di navigazione. Sfortunatamente il componente Byline non dispone di questi ma lo registreremo comunque, in quanto potrebbe essere comune per altri componenti personalizzati.

1. Aprire il modulo `ui.apps` nell&#39;IDE
1. Aprire il file `byline.html` in `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Aggiornate `byline.html` per includere l&#39;attributo `data-cmp-clickable` nell&#39;elemento **name** dell&#39;autore:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Apri un nuovo terminale. Crea e distribuisci solo il modulo `ui.apps` utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Tornate nel browser e riaprite la pagina con l’aggiunta del componente Per autore: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Per verificare l’evento, aggiungeremo manualmente alcuni JavaScript utilizzando la console di sviluppo. Per un video su come eseguire questa operazione, consultate [Utilizzo del livello dati client  Adobe con AEM componenti core](data-layer-overview.md).

1. Aprite gli strumenti di sviluppo del browser e immettete il seguente metodo nella **console**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Questo metodo semplice deve gestire il clic del nome del componente Autore.

1. Immettere il seguente metodo nella **Console**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   Il metodo precedente invia un listener di eventi sul livello dati per ascoltare l&#39;evento `cmp:click` e chiama il `bylineClickHandler`.

   >[!CAUTION]
   >
   > Sarà importante **not** aggiornare il browser durante l&#39;esercizio, altrimenti il JavaScript della console verrà perso.

1. Nel browser, con **Console** aperta, fate clic sul nome dell&#39;autore nel componente Autore:

   ![Clic su Byline Component](assets/adobe-client-data-layer/byline-component-clicked.png)

   Dovresti visualizzare il messaggio della console `Byline Clicked!` e il nome dell&#39;autore.

   L&#39;evento `cmp:click` è il più facile da collegare. Per i componenti più complessi e per tenere traccia di altri comportamenti è possibile aggiungere JavaScript personalizzato per aggiungere e registrare nuovi eventi. Un esempio lampante è il componente Carosello, che attiva un evento `cmp:show` ogni volta che una diapositiva viene attivata. Per ulteriori informazioni, vedere il codice [sorgente](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Utilizzare l&#39;utilità DataLayerBuilder {#data-layer-builder}

Quando il modello Sling era [aggiornato](#sling-model) prima nel capitolo, abbiamo scelto di creare la stringa JSON utilizzando un `HashMap` e impostando manualmente ciascuna delle proprietà. Questo metodo funziona bene per i piccoli componenti una tantum, tuttavia per i componenti che estendono i componenti core AEM questo potrebbe causare un sacco di codice aggiuntivo.

Una classe di utilità, `DataLayerBuilder`, esiste per eseguire la maggior parte del sollevamento pesante. Questo consente alle implementazioni di estendere solo le proprietà desiderate. Aggiornate il modello Sling per utilizzare il `DataLayerBuilder`.

1. Tornare all&#39;IDE e passare al modulo `core`.
1. Aprire il file `Byline.java` in `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modificare il metodo `getData()` per restituire un tipo di `ComponentData`

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` è un oggetto fornito da AEM Componenti di base. Si traduce in una stringa JSON, come nell&#39;esempio precedente, ma esegue anche un sacco di lavoro aggiuntivo.

1. Aprire il file `BylineImpl.java` in `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Aggiungete le seguenti istruzioni di importazione:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Sostituire il metodo `getData()` con quanto segue:

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   Il componente Byline riutilizza parti del componente Image Core per visualizzare un’immagine che rappresenta l’autore. Nello snippet di codice riportato sopra, [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) viene utilizzato per estendere il livello dati del componente `Image`. Questo precompila l&#39;oggetto JSON con tutti i dati relativi all&#39;immagine utilizzata. Esegue anche alcune delle funzioni di routine come l&#39;impostazione di `@type` e l&#39;identificatore univoco del componente. Notate che il metodo è davvero piccolo!

   L&#39;unica proprietà ha esteso la `withTitle` che viene sostituita con il valore di `getName()`.

1. Aprite una finestra terminale. Crea e distribuisci solo il modulo `core` utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Tornare all&#39;IDE e aprire il file `byline.html` in `ui.apps`
1. Aggiornate l&#39;HTL per utilizzare `byline.data.json` per compilare l&#39;attributo `data-cmp-data-layer`:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Ricordate che ora stiamo restituendo un oggetto di tipo `ComponentData`. Questo oggetto include un metodo getter `getJson()` che viene utilizzato per compilare l&#39;attributo `data-cmp-data-layer`.

1. Aprite una finestra terminale. Crea e distribuisci solo il modulo `ui.apps` utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Tornate nel browser e riaprite la pagina con l’aggiunta del componente Per autore: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Aprite gli strumenti di sviluppo del browser e immettete il comando seguente nella **console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Andate sotto la risposta in `component` per trovare l&#39;istanza del componente `byline`:

   ![Livello dati autore aggiornato](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Dovresti visualizzare una voce come segue:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   Tenere presente che è presente un oggetto `image` all&#39;interno della voce del componente `byline`. Contiene molte più informazioni sulla risorsa in DAM. Inoltre, `@type` e l&#39;ID univoco (in questo caso `byline-136073cfcb`) sono stati compilati automaticamente, così come l&#39; `repo:modifyDate` che indica quando il componente è stato modificato.

## Esempi aggiuntivi {#additional-examples}

1. Un altro esempio di estensione del livello dati può essere visualizzato esaminando il componente `ImageList` nella base di codici WKND:
   * `ImageList.java` - Interfaccia Java nel  `core` modulo.
   * `ImageListImpl.java` - Modello Sling nel  `core` modulo.
   * `image-list.html` - Modello HTL nel  `ui.apps` modulo.

   >[!NOTE]
   >
   > È un po&#39; più difficile includere proprietà personalizzate come `occupation` quando si utilizza [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Tuttavia, se si estende un componente di base che include un’immagine o una pagina, l’utility risparmia molto tempo.

   >[!NOTE]
   >
   > Se si crea un livello dati avanzato per gli oggetti riutilizzati durante l&#39;implementazione, si consiglia di estrarre gli elementi Livello dati nel proprio livello dati specifici Oggetti Java. Ad esempio, Commerce Core Components ha aggiunto interfacce per `ProductData` e `CategoryData`, che possono essere utilizzate su molti componenti all&#39;interno di un&#39;implementazione Commerce. Per ulteriori informazioni, consultare [il codice riportato nella repo dei componenti aem-cif-core](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer).

## Congratulazioni! {#congratulations}

Hai appena esplorato alcuni modi per estendere e personalizzare  Livello dati client Adobe con componenti AEM!

## Risorse aggiuntive {#additional-resources}

* [Documentazione sul livello dei dati del cliente  Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Integrazione dei livelli di dati con i componenti core](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Utilizzo della documentazione sul livello dati client e sui componenti core del Adobe ](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/data-layer/overview.html)
