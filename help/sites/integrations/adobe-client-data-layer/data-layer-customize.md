---
title: Personalizzare Adobe Client Data Layer con i componenti AEM
description: Scopri come personalizzare Adobe Client Data Layer con i contenuti dei componenti AEM personalizzati. Scopri come utilizzare le API fornite dai componenti core AEM per estendere e personalizzare il livello di dati.
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

Scopri come personalizzare Adobe Client Data Layer con i contenuti dei componenti AEM personalizzati. Scopri come utilizzare le API fornite da [Componenti core AEM da estendere](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) e personalizzare il livello dati.

## Cosa intendi creare

![Livello dati di Byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

In questa esercitazione esploreremo varie opzioni per estendere Adobe Client Data Layer aggiornando il WKND [Componente Byline](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html). Il _Nome autore_ il componente è un **componente personalizzato** e le lezioni apprese in questa esercitazione possono essere applicate ad altri componenti personalizzati.

### Obiettivi {#objective}

1. Inserisci i dati dei componenti nel livello dati estendendo un modello Sling e un codice HTL del componente
1. Utilizzare le utility dei livelli dati dei Componenti core per ridurre l’impegno
1. Utilizzare gli attributi di dati dei Componenti core per agganciare gli eventi dei livelli di dati esistenti

## Prerequisiti {#prerequisites}

A **ambiente di sviluppo locale** è necessario per completare questa esercitazione. Le schermate e i video vengono acquisiti mediante l’SDK as a Cloud Service per l’AEM in esecuzione su un macOS. I comandi e il codice sono indipendenti dal sistema operativo locale, salvo diversa indicazione.

**Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).

**Ti avvicini ora a AEM 6.5?** Consulta la sezione [guida seguente alla configurazione di un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Scarica e distribuisci il sito di riferimento WKND {#set-up-wknd-site}

Questa esercitazione estende il componente Byline nel sito di riferimento WKND. Clona e installa la base di codice WKND nell’ambiente locale.

1. Avvia un Quickstart locale **autore** istanza di AEM in esecuzione alle ore [http://localhost:4502](Http://localhost:4502).
1. Apri una finestra del terminale e clona la base di codice WKND utilizzando Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Distribuisci la base di codice WKND in un’istanza locale di AEM:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Per AEM 6.5 e il service pack più recente, aggiungi `classic` profilo al comando Maven:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Apri una nuova finestra del browser e accedi all’AEM. Apri un **Rivista** pagina come: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Componente Byline sulla pagina](assets/adobe-client-data-layer/byline-component-onpage.png)

   Dovresti trovare un esempio del componente Byline aggiunto alla pagina come parte di un frammento di esperienza. Puoi visualizzare il frammento di esperienza in [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Apri gli strumenti per sviluppatori e immetti il comando seguente nel **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Per visualizzare lo stato corrente del livello dati su un sito AEM, controlla la risposta. Dovresti visualizzare informazioni sulla pagina e sui singoli componenti.

   ![Adobe di risposta del livello dati](assets/data-layer-state-response.png)

   Il componente Byline non è elencato in Data Layer.

## Aggiornare il modello Sling Byline {#sling-model}

Per inserire dati sul componente nel livello dati, aggiorniamo prima il modello Sling del componente. Quindi, aggiorna l’interfaccia Java™ di Byline e l’implementazione del modello Sling per avere un nuovo metodo. `getData()`. Questo metodo contiene le proprietà da inserire nel livello dati.

1. Apri `aem-guides-wknd` progetto nell’IDE che preferisci. Accedi a `core` modulo.
1. Apri il file `Byline.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interfaccia Java Byline](assets/adobe-client-data-layer/byline-java-interface.png)

1. Aggiungi il metodo seguente all’interfaccia:

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

1. Apri il file `BylineImpl.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`. È l&#39;attuazione del `Byline` e viene implementato come modello Sling.

1. Aggiungi le seguenti istruzioni di importazione all’inizio del file:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Il `fasterxml.jackson` Le API vengono utilizzate per serializzare i dati da esporre come JSON. Il `ComponentUtils` dei Componenti core AEM vengono utilizzati per verificare se Data Layer è abilitato.

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

   Nel metodo precedente, viene visualizzata una nuova `HashMap` viene utilizzato per acquisire le proprietà da esporre come JSON. Tieni presente che i metodi esistenti come `getName()` e `getOccupations()` vengono utilizzati. Il `@type` rappresenta il tipo di risorsa univoco del componente, consente a un client di identificare facilmente eventi e/o trigger in base al tipo di componente.

   Il `ObjectMapper` viene utilizzato per serializzare le proprietà e restituire una stringa JSON. Questa stringa JSON può quindi essere inserita nel livello dati.

1. Apri una finestra del terminale. Genera e implementa solo il `core` utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Aggiornare il codice HTL della linea di base {#htl}

Quindi, aggiorna `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=en). HTL (HTML Template Language) è il modello utilizzato per eseguire il rendering del HTML del componente.

Un attributo di dati speciale `data-cmp-data-layer` su ciascun Componente AEM viene utilizzato per esporre il suo livello di dati. Il codice JavaScript fornito dai componenti core dell’AEM cerca questo attributo di dati. Il valore di questo attributo di dati viene popolato con la stringa JSON restituita dal modello Sling Byline `getData()` e inseriti nel livello dati client Adobe.

1. Apri `aem-guides-wknd` nell&#39;IDE. Accedi a `ui.apps` modulo.
1. Apri il file `byline.html` a `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Byline HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Aggiorna `byline.html` per includere `data-cmp-data-layer` attributo:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   Il valore di `data-cmp-data-layer` è stato impostato su `"${byline.data}"` dove `byline` è il modello Sling aggiornato in precedenza. `.data` è la notazione standard per chiamare un metodo Java™ Getter in HTL di `getData()` attuato nell&#39;esercizio precedente.

1. Apri una finestra del terminale. Genera e implementa solo il `ui.apps` utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Torna al browser e riapri la pagina con un componente Byline: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Apri gli strumenti per sviluppatori e controlla l’origine HTML della pagina per il componente Byline:

   ![Livello dati di Byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Dovresti vedere che `data-cmp-data-layer` è stato popolato con la stringa JSON del modello Sling.

1. Apri gli strumenti di sviluppo del browser e immetti il comando seguente nel **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Passa sotto la risposta in `component` per trovare l’istanza di `byline` il componente è stato aggiunto al livello dati:

   ![Parte Byline del livello dati](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Dovresti trovare una voce simile alla seguente:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Tieni presente che le proprietà esposte sono le stesse aggiunte nella `HashMap` nel modello Sling.

## Aggiungi un evento di clic {#click-event}

Adobe Client Data Layer è guidato dagli eventi e uno degli eventi più comuni per attivare un’azione è `cmp:click` evento. I componenti core AEM consentono di registrare facilmente il componente con l’aiuto dell’elemento dati: `data-cmp-clickable`.

Gli elementi cliccabili sono in genere un pulsante CTA o un collegamento di navigazione. Sfortunatamente, il componente Byline non ha nessuno di questi componenti, ma è comunque possibile registrarlo, in quanto potrebbe essere comune per altri componenti personalizzati.

1. Apri `ui.apps` modulo nell’IDE
1. Apri il file `byline.html` a `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Aggiorna `byline.html` per includere `data-cmp-clickable` dell&#39;attributo Byline **nome** elemento:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Apri un nuovo terminale. Genera e implementa solo il `ui.apps` utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Torna al browser e riapri la pagina con il componente Byline aggiunto: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Per testare il nostro evento, aggiungeremo manualmente un po’ di JavaScript utilizzando la console per sviluppatori. Consulta [Utilizzo di Adobe Client Data Layer con i componenti core AEM](data-layer-overview.md) per un video su come eseguire questa operazione.

1. Apri gli strumenti di sviluppo del browser e immetti il metodo seguente nel **Console**:

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

1. Immetti il seguente metodo in **Console**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   Il metodo precedente spinge un listener di eventi sul livello dati per ascoltare `cmp:click` e chiama il `bylineClickHandler`.

   >[!CAUTION]
   >
   > È importante **non** per aggiornare il browser durante questo esercizio, altrimenti il JavaScript della console andrà perso.

1. Nel browser, con **Console** Apri, fai clic sul nome dell’autore nel componente Byline:

   ![Componente Byline su cui è stato fatto clic](assets/adobe-client-data-layer/byline-component-clicked.png)

   Dovresti visualizzare il messaggio della console `Byline Clicked!` e il nome del nome del nome del nome.

   Il `cmp:click` L’evento è il più semplice da agganciare. Per componenti più complessi e per tenere traccia di altri comportamenti, è possibile aggiungere JavaScript personalizzati per aggiungere e registrare nuovi eventi. Un ottimo esempio è il componente Carosello, che attiva una `cmp:show` ogni volta che viene attivata o disattivata una diapositiva. Consulta la [codice sorgente per ulteriori dettagli](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## Utilizzare l&#39;utilità DataLayerBuilder {#data-layer-builder}

Quando il modello Sling [aggiornato](#sling-model) Nelle parti precedenti del capitolo, abbiamo scelto di creare la stringa JSON utilizzando un tag `HashMap` e impostando manualmente ciascuna proprietà. Questo metodo funziona bene per i componenti una tantum di piccole dimensioni, tuttavia per i componenti che estendono i Componenti core AEM ciò potrebbe comportare un notevole aumento del codice.

Una classe di utilità, `DataLayerBuilder`, esiste per eseguire la maggior parte del sollevamento pesante. Questo consente alle implementazioni di estendere solo le proprietà che desiderano. Aggiorniamo il modello Sling per utilizzare `DataLayerBuilder`.

1. Torna all’IDE e passa a `core` modulo.
1. Apri il file `Byline.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modifica il `getData()` metodo per restituire un tipo di `ComponentData`

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

   `ComponentData` è un oggetto fornito dai componenti core dell’AEM. Genera una stringa JSON, come nell’esempio precedente, ma esegue anche un sacco di lavoro aggiuntivo.

1. Apri il file `BylineImpl.java` a `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Aggiungi le seguenti istruzioni di importazione:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Sostituisci il `getData()` metodo con le seguenti caratteristiche:

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

   Il componente Byline riutilizza parti del componente core Immagine per visualizzare un’immagine che rappresenta l’autore. Nel frammento precedente, il [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) viene utilizzato per estendere il livello di dati del `Image` componente. L’oggetto JSON viene precompilato con tutti i dati sull’immagine utilizzata. Esegue anche alcune delle funzioni di routine come l&#39;impostazione della `@type` e l’identificatore univoco del componente. Nota che il metodo è piccolo!

   L’unica proprietà ha esteso `withTitle` che viene sostituito con il valore di `getName()`.

1. Apri una finestra del terminale. Genera e implementa solo il `core` utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Torna all’IDE e apri la `byline.html` file in `ui.apps`
1. Aggiornare HTL da utilizzare `byline.data.json` per popolare il `data-cmp-data-layer` attributo:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Ricorda che è in corso la restituzione di un oggetto di tipo `ComponentData`. Questo oggetto include un metodo getter `getJson()` e viene utilizzato per popolare il `data-cmp-data-layer` attributo.

1. Apri una finestra del terminale. Genera e implementa solo il `ui.apps` utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Torna al browser e riapri la pagina con il componente Byline aggiunto: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Apri gli strumenti di sviluppo del browser e immetti il comando seguente nel **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Passa sotto la risposta in `component` per trovare l’istanza di `byline` componente:

   ![Livello dati della linea di base aggiornato](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Dovresti trovare una voce simile alla seguente:

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

   Osserva che ora esiste un’ `image` oggetto all&#39;interno del `byline` voce del componente. Questa contiene molte più informazioni sulla risorsa in DAM. Osserva inoltre che il `@type` e l’id univoco (in questo caso `byline-136073cfcb`) sono stati compilati automaticamente e il `repo:modifyDate` che indica quando il componente è stato modificato.

## Esempi aggiuntivi {#additional-examples}

1. Un altro esempio di estensione del livello dati può essere visualizzato esaminando `ImageList` componente nella base di codice WKND:
   * `ImageList.java` - Interfaccia Java in `core` modulo.
   * `ImageListImpl.java` - Modello Sling in `core` modulo.
   * `image-list.html` - Modello HTL in `ui.apps` modulo.

   >[!NOTE]
   >
   > È un po’ più difficile includere proprietà personalizzate come `occupation` quando si utilizza [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Tuttavia, se si estende un Componente core che include un’immagine o una pagina, l’utility consente di risparmiare molto tempo.

   >[!NOTE]
   >
   > Se crei un livello dati avanzato per gli oggetti riutilizzati in un’implementazione, si consiglia di estrarre gli elementi del livello dati nei propri oggetti Java™ specifici del livello dati. Ad esempio, i componenti core Commerce hanno aggiunto interfacce per `ProductData` e `CategoryData` poiché possono essere utilizzati su molti componenti all’interno di un’implementazione Commerce. Revisione [il codice nell’archivio aem-cif-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) per ulteriori dettagli.

## Congratulazioni.  {#congratulations}

Hai appena esplorato alcuni modi per estendere e personalizzare Adobe Client Data Layer con i componenti AEM.

## Risorse aggiuntive {#additional-resources}

* [Documentazione di Adobe Client Data Layer](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Integrazione di Data Layer con i Componenti core](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Utilizzo della documentazione di Adobe Client Data Layer e Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=it)
