---
title: Personalizzare Adobe Client Data Layer con i componenti AEM
description: Scopri come personalizzare Adobe Client Data Layer con il contenuto dei componenti AEM personalizzati. Scopri come utilizzare le API fornite AEM componenti core per estendere e personalizzare il livello dati.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 99b3ecf7823ff9a116c47c88abc901f8878bbd7a
workflow-type: tm+mt
source-wordcount: '2008'
ht-degree: 2%

---

# Personalizzare Adobe Client Data Layer con i componenti AEM {#customize-data-layer}

Scopri come personalizzare Adobe Client Data Layer con il contenuto dei componenti AEM personalizzati. Scopri come utilizzare le API fornite da [AEM componenti core per estendere](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) e personalizzare il livello dati.

## Cosa verrà creato

![Livello dati byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

In questa esercitazione, esaminiamo diverse opzioni per estendere Adobe Client Data Layer aggiornando il WKND [Componente Byline](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html). La _Byline_ un componente **componente personalizzato** e le lezioni imparate in questa esercitazione possono essere applicate ad altri componenti personalizzati.

### Obiettivi {#objective}

1. Inserisci i dati dei componenti nel livello dati estendendo un modello Sling e un componente HTL
1. Utilizza le utilità dei livelli dati dei componenti core per ridurre i tempi di esecuzione
1. Utilizzare gli attributi dei dati dei componenti core per eseguire il collegamento agli eventi dei livelli dati esistenti

## Prerequisiti {#prerequisites}

A **ambiente di sviluppo locale** è necessario per completare questa esercitazione. Le schermate e i video vengono acquisiti utilizzando l’SDK AEM as a Cloud Service in esecuzione su un macOS. I comandi e il codice sono indipendenti dal sistema operativo locale, se non diversamente specificato.

**Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).

**Nuovo a AEM 6.5?** Consulta la sezione [guida alla configurazione di un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Scaricare e distribuire il sito WKND Reference {#set-up-wknd-site}

Questa esercitazione estende il componente Byline nel sito di riferimento WKND. Clona e installa la base di codice WKND nel tuo ambiente locale.

1. Avvia Quickstart locale **autore** istanza di AEM in esecuzione a [http://localhost:4502](Http://localhost:4502).
1. Apri una finestra terminale e duplica la base di codice WKND utilizzando Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Distribuisci la base di codice WKND in un&#39;istanza locale di AEM:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Per AEM 6.5 e l&#39;ultimo service pack aggiungi il `classic` profilo al comando Maven:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Apri una nuova finestra del browser e accedi a AEM. Apri un **Rivista** tipo di pagina: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Componente Byline nella pagina](assets/adobe-client-data-layer/byline-component-onpage.png)

   Dovresti visualizzare un esempio del componente Byline che è stato aggiunto alla pagina come parte di un frammento esperienza. Puoi visualizzare il frammento esperienza in [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Apri gli strumenti per sviluppatori e immetti il seguente comando nella **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Per visualizzare lo stato corrente del livello dati su un sito AEM, controlla la risposta. Dovresti visualizzare informazioni sulla pagina e sui singoli componenti.

   ![Adobe risposta livello dati](assets/data-layer-state-response.png)

   Osserva che il componente Byline non è elencato nel Livello dati.

## Aggiornare il modello Sling per riga {#sling-model}

Per inserire dati sul componente nel livello dati, prima di tutto aggiorniamo il modello Sling del componente. Quindi, aggiorna l&#39;interfaccia Java™ di Byline e l&#39;implementazione di Sling Model per avere un nuovo metodo `getData()`. Questo metodo contiene le proprietà da inserire nel livello dati.

1. Apri `aem-guides-wknd` nell’IDE che preferisci. Passa a `core` modulo .
1. Apri il file . `Byline.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interfaccia Java Byline](assets/adobe-client-data-layer/byline-java-interface.png)

1. Aggiungi il metodo sottostante all&#39;interfaccia:

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

1. Apri il file . `BylineImpl.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`. È l&#39;attuazione `Byline` e viene implementato come modello Sling.

1. Aggiungi le seguenti istruzioni di importazione all’inizio del file :

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   La `fasterxml.jackson` Le API vengono utilizzate per serializzare i dati da esporre come JSON. La `ComponentUtils` di AEM componenti core vengono utilizzati per verificare se il livello dati è abilitato.

1. Aggiungi il metodo non implementato `getData()` a `BylineImple.java`:

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

   Nel metodo precedente, un nuovo `HashMap` viene utilizzato per acquisire le proprietà da esporre come JSON. Osserva che i metodi esistenti come `getName()` e `getOccupations()` sono utilizzati. La `@type` rappresenta il tipo di risorsa univoco del componente, che consente a un client di identificare facilmente gli eventi e/o i trigger in base al tipo di componente.

   La `ObjectMapper` viene utilizzato per serializzare le proprietà e restituire una stringa JSON. Questa stringa JSON può quindi essere inserita nel livello dati.

1. Apri una finestra terminale. Crea e implementa solo le `core` modulo con le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Aggiornare l’HTL per riga precedente {#htl}

Quindi, aggiorna il `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=en). HTL (HTML Template Language) è il modello utilizzato per eseguire il rendering del HTML del componente.

Attributo dati speciale `data-cmp-data-layer` in ogni componente AEM viene utilizzato per esporre il livello dati. JavaScript fornito da AEM componenti core cerca questo attributo di dati. Il valore di questo attributo di dati viene popolato con la stringa JSON restituita dal modello Byline Sling `getData()` e inserito in Adobe Client Data Layer.

1. Apri `aem-guides-wknd` proiettare nell’IDE. Passa a `ui.apps` modulo .
1. Apri il file . `byline.html` a `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![HTML autore](assets/adobe-client-data-layer/byline-html-template.png)

1. Aggiorna `byline.html` per includere `data-cmp-data-layer` attributo:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   Il valore di `data-cmp-data-layer` è impostato su `"${byline.data}"` dove `byline` è il modello Sling aggiornato in precedenza. `.data` è la notazione standard per la chiamata di un metodo Java™ Getter in HTL di `getData()` attuato nell&#39;esercizio precedente.

1. Apri una finestra terminale. Crea e implementa solo le `ui.apps` modulo con le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Torna al browser e riapri la pagina con un componente Byline: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Apri gli strumenti per sviluppatori ed esamina l’origine HTML della pagina per il componente Byline:

   ![Livello dati byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Dovresti vedere che la `data-cmp-data-layer` è stato popolato con la stringa JSON dal modello Sling.

1. Apri gli strumenti di sviluppo del browser e immetti il seguente comando nella **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Naviga sotto la risposta in `component` per trovare l&#39;istanza del `byline` componente è stato aggiunto al livello dati:

   ![Byline part del livello dati](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Dovresti visualizzare una voce come segue:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Osserva che le proprietà esposte sono le stesse aggiunte nel `HashMap` nel modello Sling.

## Aggiungi un evento clic {#click-event}

Adobe Client Data Layer è basato su eventi e uno degli eventi più comuni per attivare un&#39;azione è la `cmp:click` evento. I componenti core AEM facilitano la registrazione del componente con l’aiuto dell’elemento dati: `data-cmp-clickable`.

Gli elementi selezionabili sono in genere un pulsante CTA o un collegamento di navigazione. Sfortunatamente il componente Byline non dispone di nessuno di questi ma lo registreremo comunque, in quanto potrebbe essere comune per altri componenti personalizzati.

1. Apri `ui.apps` modulo nell’IDE
1. Apri il file . `byline.html` a `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Aggiorna `byline.html` per includere `data-cmp-clickable` Attributo su Byline&#39;s **name** elemento:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Apri un nuovo terminale. Crea e implementa solo le `ui.apps` modulo con le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Torna al browser e riapri la pagina con il componente Byline aggiunto: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Per testare il nostro evento, aggiungeremo manualmente alcuni JavaScript utilizzando la console per sviluppatori. Vedi [Utilizzo di Adobe Client Data Layer con AEM componenti core](data-layer-overview.md) per un video su come eseguire questa procedura.

1. Apri gli strumenti di sviluppo del browser e immetti il seguente metodo in **Console**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Questo metodo semplice deve gestire il clic del nome del componente Byline.

1. Inserisci il seguente metodo nel **Console**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   Il metodo di cui sopra spinge un listener di eventi sul livello dati per ascoltare `cmp:click` e chiama `bylineClickHandler`.

   >[!CAUTION]
   >
   > È importante **not** per aggiornare il browser durante questo esercizio, altrimenti la console JavaScript viene persa.

1. Nel browser, con **Console** apri, fai clic sul nome dell’autore nel componente Byline :

   ![Clic su Byline Component](assets/adobe-client-data-layer/byline-component-clicked.png)

   Dovresti visualizzare il messaggio della console `Byline Clicked!` e il nome della riga.

   La `cmp:click` l&#39;evento è il più semplice in cui collegarsi. Per i componenti più complessi e per tenere traccia di altri comportamenti è possibile aggiungere JavaScript personalizzati per aggiungere e registrare nuovi eventi. Un esempio lampante è il componente Carosello, che attiva un `cmp:show` ogni volta che una diapositiva viene attivata. Consulta la sezione [codice sorgente per ulteriori dettagli](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## Utilizzare l&#39;utility DataLayerBuilder {#data-layer-builder}

Quando il modello Sling era [aggiornato](#sling-model) in precedenza nel capitolo, abbiamo scelto di creare la stringa JSON utilizzando un `HashMap` e impostare manualmente ciascuna proprietà. Questo metodo funziona bene per i componenti una tantum di piccole dimensioni, tuttavia per i componenti che estendono i componenti core di AEM questo potrebbe causare un sacco di codice aggiuntivo.

Una classe di utilità, `DataLayerBuilder`, esiste per eseguire la maggior parte del sollevamento pesante. Questo consente alle implementazioni di estendere solo le proprietà desiderate. Aggiorna il modello Sling per utilizzare il `DataLayerBuilder`.

1. Torna all’IDE e passa alla `core` modulo .
1. Apri il file . `Byline.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modifica la `getData()` metodo per restituire un tipo di `ComponentData`

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

   `ComponentData` è un oggetto fornito AEM componenti core. Si traduce in una stringa JSON, come nell’esempio precedente, ma esegue anche molto lavoro aggiuntivo.

1. Apri il file . `BylineImpl.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Aggiungi le seguenti istruzioni di importazione:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Sostituisci il `getData()` con il seguente metodo:

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

   Il componente Byline riutilizza parti del componente di base immagine per visualizzare un’immagine che rappresenta l’autore. Nello snippet di codice riportato sopra, il [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) viene utilizzato per estendere il livello dati del `Image` componente. Questo precompila l’oggetto JSON con tutti i dati sull’immagine utilizzata. Esegue anche alcune delle funzioni di routine come l&#39;impostazione del `@type` e l&#39;identificatore univoco del componente. Notate che il metodo è piccolo!

   L&#39;unica proprietà estesa `withTitle` che viene sostituito con il valore di `getName()`.

1. Apri una finestra terminale. Crea e implementa solo le `core` modulo con le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Torna all’IDE e apri la `byline.html` file in `ui.apps`
1. Aggiornare HTL per utilizzare `byline.data.json` per popolare `data-cmp-data-layer` attributo:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Ricorda che ora stiamo restituendo un oggetto di tipo `ComponentData`. Questo oggetto include un metodo getter `getJson()` e viene utilizzato per popolare il `data-cmp-data-layer` attributo.

1. Apri una finestra terminale. Crea e implementa solo le `ui.apps` modulo con le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Torna al browser e riapri la pagina con il componente Byline aggiunto: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Apri gli strumenti di sviluppo del browser e immetti il seguente comando nella **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Naviga sotto la risposta in `component` per trovare l&#39;istanza del `byline` componente:

   ![Livello dati byline aggiornato](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   Osserva che ora esiste un `image` all&#39;interno dell&#39;oggetto `byline` voce del componente. Questo contiene molte più informazioni sulla risorsa in DAM. Osserva anche che `@type` e l&#39;id univoco (in questo caso `byline-136073cfcb`) sono stati compilati automaticamente e le `repo:modifyDate` indica quando il componente è stato modificato.

## Esempi aggiuntivi {#additional-examples}

1. Un altro esempio di estensione del livello dati può essere visualizzato analizzando il `ImageList` componente nella base di codice WKND:
   * `ImageList.java` - Interfaccia Java nel `core` modulo .
   * `ImageListImpl.java` - Modello Sling in `core` modulo .
   * `image-list.html` - Modello HTL nel `ui.apps` modulo .

   >[!NOTE]
   >
   > È un po&#39; più difficile includere proprietà personalizzate come `occupation` quando si utilizza [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Tuttavia, se estendi un componente di base che include un&#39;immagine o una pagina, l&#39;utility risparmia molto tempo.

   >[!NOTE]
   >
   > Se si crea un livello dati avanzato per gli oggetti riutilizzati durante un&#39;implementazione, si consiglia di estrarre gli elementi del livello dati nei propri oggetti Java™ specifici per il livello dati. Ad esempio, i componenti core Commerce hanno aggiunto interfacce per `ProductData` e `CategoryData` poiché potrebbero essere utilizzati su molti componenti all’interno di un’implementazione Commerce. Revisione [il codice nell’archivio dei componenti aem-cif-core](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) per ulteriori dettagli.

## Congratulazioni.  {#congratulations}

Hai appena esplorato alcuni modi per estendere e personalizzare Adobe Client Data Layer con componenti AEM!

## Risorse aggiuntive {#additional-resources}

* [Documentazione di Adobe Client Data Layer](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Integrazione dei livelli dati con i componenti core](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Utilizzo della documentazione di Adobe Client Data Layer e Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it)
