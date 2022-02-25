---
title: Componente personalizzato
description: Copre la creazione end-to-end di un componente personalizzato per riga che visualizza il contenuto creato. Include lo sviluppo di un modello Sling per incapsulare la logica di business per popolare il componente byline e l’HTL corrispondente per eseguire il rendering del componente.
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '4138'
ht-degree: 0%

---

# Componente personalizzato {#custom-component}

Questa esercitazione descrive la creazione end-to-end di un componente Byline AEM personalizzato che visualizza i contenuti creati in una finestra di dialogo ed esplora lo sviluppo di un modello Sling per incapsulare la logica di business che popola l’HTL del componente.

## Prerequisiti {#prerequisites}

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Progetto iniziale

>[!NOTE]
>
> Se hai completato con successo il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Consulta la sezione `tutorial/custom-component-start` ramo [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi la variabile `classic` su qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) o controlla il codice localmente passando al ramo `tutorial/custom-component-solution`.

## Obiettivo

1. Come creare un componente AEM personalizzato
1. Scopri come incapsulare la logica di business con i modelli Sling
1. Come utilizzare un modello Sling all’interno di uno script HTL

## Cosa verrà creato {#byline-component}

In questa parte dell’esercitazione WKND, viene creato un componente Byline che verrà utilizzato per visualizzare le informazioni create sul collaboratore di un articolo.

![esempio di componente byline](assets/custom-component/byline-design.png)

*Componente Byline*

L’implementazione del componente Byline include una finestra di dialogo che raccoglie il contenuto del byline e un modello Sling personalizzato che recupera il contenuto del byline:

* Nome
* Immagine
* Occupazioni

## Crea componente Byline {#create-byline-component}

Innanzitutto, crea la struttura del nodo del componente Byline e definisci una finestra di dialogo. Rappresenta il componente in AEM e definisce implicitamente il tipo di risorsa del componente in base alla sua posizione nel JCR.

La finestra di dialogo espone l’interfaccia con cui gli autori possono fornire i contenuti. Per questa implementazione, il componente di base WCM AEM **Immagine** verrà utilizzato per gestire l’authoring e il rendering dell’immagine del componente Byline, in modo che venga impostato come componente `sling:resourceSuperType`.

### Crea definizione componente {#create-component-definition}

1. In **ui.apps** modulo, passa a `/apps/wknd/components` e crea una nuova cartella denominata `byline`.
1. Sotto `byline` aggiungi un nuovo file denominato `.content.xml`

   ![finestra di dialogo per creare un nodo](assets/custom-component/byline-node-creation.png)

1. Popolare `.content.xml` file con le seguenti caratteristiche:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   Il file XML di cui sopra fornisce la definizione del componente, inclusi il titolo, la descrizione e il gruppo. La `sling:resourceSuperType` punti `core/wcm/components/image/v2/image`, che è [Componente immagine core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html).

### Creare lo script HTL {#create-the-htl-script}

1. Sotto `byline` cartella, aggiungi un nuovo file `byline.html`, responsabile della presentazione HTML del componente. È importante denominare il file come cartella, poiché diventa lo script predefinito che Sling utilizzerà per eseguire il rendering di questo tipo di risorsa.

1. Aggiungi il seguente codice al `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` è [rivisto più tardi](#byline-htl)una volta creato il modello Sling. Lo stato corrente del file HTL consente al componente di essere visualizzato in uno stato vuoto, nell’Editor pagina di AEM Sites quando viene trascinato e rilasciato sulla pagina.

### Creare la definizione della finestra di dialogo {#create-the-dialog-definition}

Quindi, definisci una finestra di dialogo per il componente Byline con i campi seguenti:

* **Nome**: un campo di testo con il nome del collaboratore.
* **Immagine**: un riferimento alla foto biografica del collaboratore.
* **Occupazioni**: un elenco delle professioni attribuite al collaboratore. Le professioni devono essere ordinate alfabeticamente in ordine crescente (da a a z).

1. Sotto `byline` creare una nuova cartella denominata `_cq_dialog`.
1. Sotto `byline/_cq_dialog` aggiungi un nuovo file denominato `.content.xml`. Definizione XML per la finestra di dialogo. Aggiungi il seguente XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
                       <properties
                               jcr:primaryType="nt:unstructured"
                               jcr:title="Properties"
                               sling:resourceType="granite/ui/components/coral/foundation/container"
                               margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                       jcr:primaryType="nt:unstructured"
                                       sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                       margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                               jcr:primaryType="nt:unstructured"
                                               sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   Queste definizioni dei nodi di dialogo utilizzano [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) per controllare quali schede di dialogo vengono ereditate dal `sling:resourceSuperType` in questo caso il **Componente Immagine dei componenti core**.

   ![finestra di dialogo completata per nome](assets/custom-component/byline-dialog-created.png)

### Finestra di dialogo Crea criterio {#create-the-policy-dialog}

Seguendo lo stesso approccio della creazione della finestra di dialogo, crea una finestra di dialogo Criteri (precedentemente nota come finestra di dialogo di progettazione) per nascondere i campi indesiderati nella configurazione dei criteri ereditati dal componente Immagine dei componenti core.

1. Sotto `byline` creare una nuova cartella denominata `_cq_design_dialog`.
1. Sotto `byline/_cq_design_dialog` crea un nuovo file denominato `.content.xml`. Aggiorna il file con quanto segue: con il seguente XML. È più facile aprire il `.content.xml` e copia/incolla il codice XML sottostante.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   La base per la **Finestra di dialogo dei criteri** XML ottenuto da [Componente Immagine dei componenti core](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Come nella configurazione della finestra di dialogo, [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) viene utilizzato per nascondere campi non rilevanti altrimenti ereditati dalla `sling:resourceSuperType`, come mostrato dalle definizioni dei nodi con `sling:hideResource="{Boolean}true"` proprietà.

### Distribuire il codice {#deploy-the-code}

1. Sincronizza le modifiche in `ui.apps` con il tuo IDE o utilizzando le tue competenze Maven.

   ![Esportazione in AEM componente per riga di riga del server](assets/custom-component/export-byline-component-aem.png)

## Aggiungere il componente a una pagina {#add-the-component-to-a-page}

Per semplificare e concentrarsi sullo sviluppo AEM componente, aggiungiamo il componente Byline nel suo stato corrente a una pagina Articolo per verificare il `cq:Component` la definizione del nodo è implementata e corretta, AEM riconosce la nuova definizione del componente e la finestra di dialogo del componente funziona per l’authoring.

### Aggiungere un’immagine ad AEM Assets

Innanzitutto, carica un campione di head shot in AEM Assets da usare per popolare l&#39;immagine nel componente Byline.

1. Passa alla cartella LA Skateparks in AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Carica la foto della testa per  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** nella cartella.

   ![Headshot caricato in AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Creare il componente {#author-the-component}

Quindi, aggiungi il componente Byline a una pagina in AEM. Poiché è stato aggiunto il componente Byline al **Progetto WKND Sites - Contenuto** Gruppo di componenti, tramite `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` è automaticamente disponibile per qualsiasi **Contenitore** di cui **Criterio** consente di **Progetto WKND Sites - Contenuto** gruppo di componenti, che rappresenta il Contenitore di layout della pagina dell’articolo.

1. Passa all&#39;articolo LO Skatepark all&#39;indirizzo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Dalla barra laterale sinistra, trascina e rilascia una **Componente Byline** su **bottom** del Contenitore di layout della pagina dell’articolo aperta.

   ![aggiungi componente per riga nella pagina](assets/custom-component/add-to-page.png)

1. Assicurati che **barra laterale sinistra aperta** e visibili, e **Ricerca risorse** è selezionato.

1. Seleziona la **Segnaposto componente bobina**, che a sua volta visualizza la barra delle azioni e tocca il **chiave** per aprire la finestra di dialogo.

1. Con la finestra di dialogo aperta e la prima scheda (Risorsa) attiva, apri la barra laterale sinistra e, da Asset Finder, trascina un’immagine nella zona di rilascio Immagine. Cerca &quot;stacey&quot; per trovare Stacey Roswells bio picture fornito nel pacchetto WKND ui.content.

   ![aggiungi immagine alla finestra di dialogo](assets/custom-component/add-image.png)

1. Dopo aver aggiunto un’immagine, fai clic sul pulsante **Proprietà** scheda per accedere al **Nome** e **Occupazioni**.

   Entrando in occupazioni, inseriscili **alfabetico inverso** l’ordine in modo che la logica di business dell’alfabeto che implementeremo nel modello Sling sia immediatamente evidente.

   Tocca **Fine** in basso a destra per salvare le modifiche.

   ![popolare le proprietà del componente byline](assets/custom-component/add-properties.png)

   AEM gli autori configurano e creano i componenti tramite le finestre di dialogo. A questo punto nello sviluppo del componente Byline le finestre di dialogo sono incluse per la raccolta dei dati, tuttavia la logica per il rendering del contenuto creato non è ancora stata aggiunta. Viene quindi visualizzato solo il segnaposto.

1. Dopo aver salvato la finestra di dialogo, passa a [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) e controlla in che modo il contenuto del componente viene memorizzato nel nodo del contenuto del componente per riga, sotto la pagina di AEM.

   Trova il nodo del contenuto del componente Byline sotto la pagina Skate Parks di LA, ovvero `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Osserva i nomi delle proprietà `name`, `occupations`e `fileReference` sono memorizzati sul **nodo secondario**.

   Inoltre, noterai `sling:resourceType` del nodo è impostato su `wknd/components/content/byline` che è ciò che lega questo nodo di contenuto all’implementazione del componente Byline.

   ![proprietà byline in CRXDE](assets/custom-component/byline-properties-crxde.png)

## Crea modello Sling per riga {#create-sling-model}

Ora creeremo un modello Sling per fungere da modello dati e ospitare la logica di business per il componente Byline.

I modelli Sling sono Java &quot;POJO&quot; (Plain Old Java Objects) basati su annotazioni che facilitano la mappatura dei dati da JCR alle variabili Java e forniscono una serie di altri concetti quando si sviluppano nel contesto di AEM.

### Rivedi dipendenze Maven {#maven-dependency}

Il modello Sling Byline si basa su diverse API Java fornite da AEM. Queste API sono rese disponibili tramite il `dependencies` elencati nel `core` file POM del modulo. Il progetto utilizzato per questa esercitazione è stato creato per AEM as a Cloud Service. Tuttavia è unico in quanto è retrocompatibile con AEM 6.5/6.4. Pertanto sono incluse entrambe le dipendenze per Cloud Service e AEM 6.x.

1. Apri `pom.xml` file sottostante `<src>/aem-guides-wknd/core/pom.xml`.
1. Trova la dipendenza per `aem-sdk-api` - **Solo AEM as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   La [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#building-for-the-sdk) contiene tutte le API Java pubbliche esposte da AEM. La `aem-sdk-api` viene utilizzato per impostazione predefinita durante la creazione del progetto. La versione viene mantenuta nel riquadro del reattore principale situato nella directory principale del progetto in `aem-guides-wknd/pom.xml`.

1. Trova la dipendenza per `uber-jar` - **Solo AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   La `uber-jar` è incluso solo quando `classic` viene richiamato il profilo, ovvero `mvn clean install -PautoInstallSinglePackage -Pclassic`. Anche in questo caso, questo è unico per questo progetto. In un progetto del mondo reale, generato dall’Archetipo di progetto AEM `uber-jar` sarà l&#39;impostazione predefinita se la versione di AEM specificata è 6.5 o 6.4.

   La [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contiene tutte le API Java pubbliche esposte da AEM 6.x. La versione viene mantenuta nel riquadro del reattore principale situato nella directory principale del progetto `aem-guides-wknd/pom.xml`.

1. Trova la dipendenza per `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Tutte le API Java pubbliche esposte dai componenti core AEM. AEM componenti core è un progetto gestito al di fuori di AEM e dispone quindi di un ciclo di rilascio separato. Per questo motivo è una dipendenza che deve essere inclusa separatamente ed è **not** incluso con `uber-jar` o `aem-sdk-api`.

   Come il file uber-jar, la versione per questa dipendenza viene mantenuta nel file pom del reattore principale che si trova in `aem-guides-wknd/pom.xml`.

   Più avanti in questa esercitazione utilizzeremo la classe immagine del componente core per visualizzare l’immagine nel componente Byline. È necessario avere la dipendenza del componente core per generare e compilare il nostro modello Sling.

### Interfaccia ibrida {#byline-interface}

Crea un&#39;interfaccia Java pubblica per il comando Byline. `Byline.java` definisce i metodi pubblici necessari per guidare `byline.html` Script HTL.

1. All&#39;interno di `aem-guides-wknd.core` modulo sottostante `core/src/main/java/com/adobe/aem/guides/wknd/core/models` crea un nuovo file denominato `Byline.java`

   ![creare un&#39;interfaccia per riga](assets/custom-component/create-byline-interface.png)

1. Aggiorna `Byline.java` con i seguenti metodi:

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   I primi due metodi espongono i valori per **name** e **professioni** per il componente Byline.

   La `isEmpty()` viene utilizzato per determinare se il componente ha del contenuto da riprodurre o se è in attesa di essere configurato.

   Nota che non esiste un metodo per l&#39;immagine; [daremo un&#39;occhiata al motivo per cui questo è più tardi](#tackling-the-image-problem).

1. I pacchetti Java che contengono classi Java pubbliche, in questo caso un modello Sling, devono essere sottoposti a una versione utilizzando il pacchetto  `package-info.java` file.

   Dal pacchetto Java della sorgente WKND `com.adobe.aem.guides.wknd.core.models` dichiara è la versione di `1.0.0`e stiamo aggiungendo un’interfaccia e metodi pubblici unificanti, la versione deve essere aumentata a `1.1.0`. Apri il file in `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` e aggiorna `@Version("1.0.0")` a `@Version("1.1.0")`.

       &quot;
       @Version(&quot;2.1.0&quot;)
       package com.adobe.aem.guides.wknd.core.models;
       
       importa org.osgi.annotation.versioning.Version;
       &quot;
   
   Ogni volta che viene apportata una modifica ai file di questo pacchetto, il [la versione del pacchetto deve essere regolata semanticamente](https://semver.org/). In caso contrario, il progetto Maven è [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) rileva una versione del pacchetto non valida e interrompe la generazione. Fortunatamente, in caso di errore, il plug-in Maven segnala la versione non valida del pacchetto Java e la versione che dovrebbe essere. Ho appena aggiornato il `@Version("...")` Dichiarazione nel pacchetto Java che viola `package-info.java` alla versione consigliata dal plug-in da correggere.

### Implementazione byline {#byline-implementation}

`BylineImpl.java` è l’implementazione del modello Sling che implementa il `Byline.java` interfaccia definita in precedenza. Il codice completo per `BylineImpl.java` si trova in fondo a questa sezione.

1. Crea una nuova cartella denominata `impl` sotto `core/src/main/java/com/adobe/aem/guides/core/models`.
1. In `impl` creare un nuovo file `BylineImpl.java`.

   ![File Impl del banner](assets/custom-component/byline-impl-file.png)

1. Apri `BylineImpl.java`. Specifica che implementa il `Byline` interfaccia. Utilizza le funzioni di completamento automatico dell’IDE o aggiorna manualmente il file per includere i metodi necessari per implementare il `Byline` interfaccia:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. Aggiungere le annotazioni del modello Sling aggiornando `BylineImpl.java` con le seguenti annotazioni a livello di classe. Questo `@Model(..)`annotazione è ciò che trasforma la classe in un modello Sling.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   Esaminiamo questa annotazione e i relativi parametri:

   * La `@Model` l’annotazione registra BylineImpl come modello Sling quando viene implementato in AEM.
   * La `adaptables` specifica che questo modello può essere adattato dalla richiesta.
   * La `adapters` Il parametro consente di registrare la classe di implementazione sotto l&#39;interfaccia Byline. Questo consente allo script HTL di chiamare il modello Sling tramite l’interfaccia (invece dell’impl direttamente). [Maggiori dettagli sugli adattatori sono disponibili qui](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * La `resourceType` punta al tipo di risorsa del componente Byline (creato in precedenza) e aiuta a risolvere il modello corretto in presenza di più implementazioni. [Ulteriori dettagli sull&#39;associazione di una classe modello a un tipo di risorsa sono disponibili qui](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementazione dei metodi del modello Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

Il primo metodo che affronteremo è `getName()` che restituisce semplicemente il valore memorizzato nel nodo di contenuto JCR del byline sotto la proprietà `name`.

Per questo, la `@ValueMapValue` L’annotazione Modello Sling viene utilizzata per inserire il valore in un campo Java utilizzando ValueMap della risorsa Request.


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

Poiché la proprietà JCR condivide lo stesso nome del campo Java (entrambi sono &quot;name&quot;), `@ValueMapValue` risolve automaticamente questa associazione e inserisce il valore della proprietà nel campo Java.

#### getOccupations() {#implementing-get-occupations}

Il metodo successivo da implementare è `getOccupations()`. Questo metodo raccoglie tutte le occupazioni memorizzate nella proprietà JCR `occupations` e restituiscono una raccolta ordinata (in ordine alfabetico).

Utilizzando la stessa tecnica esplorata in `getName()` il valore della proprietà può essere inserito nel campo del modello Sling.

Una volta che i valori delle proprietà JCR sono disponibili nel modello Sling tramite il campo Java inserito `occupations`, la logica di business di ordinamento può essere applicata nella `getOccupations()` metodo .


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

L&#39;ultimo metodo pubblico è `isEmpty()` che determina quando il componente deve considerare se stesso &quot;abbastanza creato&quot; per il rendering.

Per questo componente, abbiamo requisiti aziendali che indicano che tutti e tre i campi, nome, immagine e occupazioni devono essere compilati *prima* è possibile eseguire il rendering del componente.


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### Affrontare il &quot;problema dell&#39;immagine&quot; {#tackling-the-image-problem}

Controllare il nome e le condizioni di occupazione sono banali (e l&#39;Apache Commons Lang3 fornisce sempre il [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) (Classe), tuttavia, non è chiaro come **presenza dell&#39;immagine** può essere convalidato in quanto il componente Immagine dei componenti core viene utilizzato per rendere l’immagine di superficie.

Ci sono due modi per affrontare questo problema:

Controlla se la `fileReference` La proprietà JCR viene risolta in una risorsa. *O* Converti questa risorsa in un modello Sling per immagini di componente core e assicurati che `getSrc()` metodo non vuoto.

Sceglieremo per **secondo** approccio. Il primo approccio è probabilmente sufficiente, ma in questa esercitazione quest’ultima ci permetterà di esplorare altre funzionalità di Sling Models.

1. Crea un metodo privato per ottenere l&#39;immagine. Questo metodo viene lasciato privato perché non è necessario esporre l&#39;oggetto Immagine nell&#39;HTL stesso e il suo solo utilizzo per l&#39;unità `isEmpty().`

   Aggiungi il seguente metodo privato per `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Come indicato sopra, ci sono altri due approcci per ottenere il **Modello Sling immagine**:

   Il primo utilizza il `@Self` annotazione, per adattare automaticamente la richiesta corrente al componente core `Image.class`

   Il secondo utilizza il [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) Il servizio OSGi, che è un servizio molto utile, e ci aiuta a creare modelli Sling di altri tipi nel codice Java.

   Sceglieremo il secondo approccio.

   >[!NOTE]
   >
   >In un’implementazione reale, adotta l’approccio &quot;Uno&quot;, utilizzando `@Self` è da preferirsi perché è la soluzione più semplice ed elegante. In questa esercitazione utilizzeremo il secondo approccio, in quanto ci richiede di esplorare più facet di modelli Sling che sono estremamente utili sono componenti più complessi!

   Poiché i modelli Sling sono Java POJO e non OSGi Services, le consuete annotazioni di iniezione OSGi `@Reference` **impossibile** , invece i modelli Sling forniscono un **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** annotazione che fornisce funzionalità simili.

1. Aggiorna `BylineImpl.java` per includere `OSGiService` per inserire l’annotazione `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   Con la `ModelFactory` disponibile, è possibile creare un modello Sling immagine componente core utilizzando:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Tuttavia, questo metodo richiede sia una richiesta che una risorsa, non ancora disponibili nel modello Sling. Per ottenere questi risultati, vengono utilizzate più annotazioni modello Sling!

   Per ottenere la richiesta corrente **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** può essere utilizzata per inserire `adaptable` (definito nella `@Model(..)` come `SlingHttpServletRequest.class`, in un campo classe Java.

1. Aggiungi il **@Self** per ottenere l’annotazione **Richiesta SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Ricorda di utilizzare `@Self Image image` per inserire il modello Sling immagine del componente core era un’opzione precedente - il `@Self` annotation cerca di inserire l&#39;oggetto adattabile (nel nostro caso un SlingHttpServletRequest) e di adattarsi al tipo di campo di annotazione. Poiché il modello Sling per immagini del componente core è adattabile dagli oggetti SlingHttpServletRequest, questo avrebbe funzionato ed è meno codice del nostro approccio più esplorativo.

   Ora abbiamo inserito le variabili necessarie per creare un&#39;istanza del nostro modello Immagine tramite l&#39;API ModelFactory. Useremo i modelli Sling **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** annotazione per ottenere questo oggetto dopo la creazione dell&#39;istanza Sling Model.

   `@PostConstruct` è incredibilmente utile e agisce in una capacità simile a quella di un costruttore, tuttavia viene richiamato dopo che la classe è stata creata un&#39;istanza e tutti i campi Java annotati vengono inseriti. considerando che altre annotazioni del modello Sling annotano campi della classe Java (variabili), `@PostConstruct` annota un metodo del parametro void, zero, generalmente denominato `init()` (ma può essere chiamato qualsiasi cosa).

1. Aggiungi **@PostConstruct** metodo:

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   Ricorda che i modelli Sling sono **NOT** Servizi OSGi, quindi è sicuro mantenere lo stato di classe. Spesso `@PostConstruct` deriva e imposta lo stato della classe del modello Sling per un uso successivo, simile a quello di un costruttore normale.

   Tieni presente che se `@PostConstruct` restituisce un&#39;eccezione. L&#39;istanza del modello Sling non verrà creata (sarà null).

1. **getImage()** può ora essere aggiornato per restituire semplicemente l’oggetto immagine.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Torniamo a `isEmpty()` e completa l&#39;implementazione:

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   Nota chiamate multiple a `getImage()` non è problematico perché restituisce l&#39;inizializzato `image` variabile di classe e non richiama `modelFactory.getModelFromWrappedRequest(...)` che non è troppo costoso, ma vale la pena evitare di chiamare inutilmente.

1. Il finale `BylineImpl.java` dovrebbe essere simile a:


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that will be used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## Byline HTL {#byline-htl}

In `ui.apps` modulo, aperto `/apps/wknd/components/byline/byline.html` creato nella configurazione precedente del componente AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Rivediamo cosa fa finora questo script HTL:

* La `placeholderTemplate` fa riferimento al segnaposto dei componenti core, visualizzato quando il componente non è completamente configurato. Viene eseguito il rendering nell’Editor pagina di AEM Sites come una casella con il titolo del componente, come definito in precedenza nel `cq:Component`s  `jcr:title` proprietà.

* La `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carica il `placeholderTemplate` definito sopra e trasmette un valore booleano (attualmente codificato in `false`) nel modello segnaposto. Quando `isEmpty` è vero, il modello segnaposto esegue il rendering della casella grigia, altrimenti non esegue alcun rendering.

### Aggiorna HTL byline

1. Aggiorna **byline.html** con la seguente struttura di HTML scheletro:

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Nota che le classi CSS seguono le [Convenzione di denominazione BEM](https://getbem.com/naming/). Anche se l’utilizzo delle convenzioni BEM non è obbligatorio, BEM è consigliato in quanto è utilizzato nelle classi CSS dei componenti core e generalmente genera regole CSS pulite e leggibili.

### Creazione di un’istanza di oggetti modello Sling in HTL {#instantiating-sling-model-objects-in-htl}

La [Istruzione block](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) viene utilizzato per creare un&#39;istanza degli oggetti modello Sling nello script HTL e assegnarlo a una variabile HTL.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utilizza l’interfaccia Byline (com.adobe.aem.guides.wknd.models.Byline) implementata da BylineImpl e adatta l’attuale SlingHttpServletRequest e il risultato viene memorizzato in un nome di variabile HTL per riga ( `data-sly-use.<variable-name>`).

1. Aggiorna l&#39;esterno `div` per fare riferimento al **Byline** Modello Sling dalla sua interfaccia pubblica:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Accesso ai metodi del modello Sling {#accessing-sling-model-methods}

HTL prende in prestito da JSTL e utilizza lo stesso accorciamento dei nomi dei metodi getter Java.

Ad esempio, richiamando il modello Sling Byline `getName()` può essere abbreviato in `byline.name`, in modo simile anziché `byline.isEmpty`, può essere abbreviato in `byline.empty`. Utilizzando nomi completi dei metodi, `byline.getName` o `byline.isEmpty`, funziona anche. Tieni presente che `()` non vengono mai utilizzati per richiamare metodi in HTL (simili a JSTL).

Metodi Java che richiedono un parametro **impossibile** da utilizzare in HTL. Questo è di progettazione per mantenere la logica in HTL semplice.

1. Il nome della riga può essere aggiunto al componente richiamando il `getName()` sul modello Byline Sling, o in HTL: `${byline.name}`.

   Aggiorna `h2` tag:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Utilizzo delle opzioni di espressione HTL {#using-htl-expression-options}

[Opzioni delle espressioni HTL](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) agisce come modificatore sul contenuto in HTL e può variare dalla formattazione della data alla traduzione i18n. Le espressioni possono anche essere utilizzate per unire elenchi o array di valori, che è ciò che è necessario per visualizzare le occupazioni in un formato delimitato da virgole.

Le espressioni vengono aggiunte tramite il `@` nell’espressione HTL.

1. Per unire l’elenco delle occupazioni con &quot;, &quot;, viene utilizzato il seguente codice:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Visualizzazione condizionale del segnaposto {#conditionally-displaying-the-placeholder}

La maggior parte degli script HTL per i componenti AEM sfrutta **paradigma segnaposto** per fornire un segnale visivo agli autori **indica che un componente è creato in modo non corretto e non verrà visualizzato in AEM Publish**. La convenzione per guidare questa decisione è quella di implementare un metodo sul modello Sling di supporto del componente, nel nostro caso: `Byline.isEmpty()`.

`isEmpty()` viene richiamato sul modello Sling Byline e il risultato (o meglio il suo negativo, tramite `!` viene salvato in una variabile HTL denominata `hasContent`:

1. Aggiorna l&#39;esterno `div` per salvare una variabile HTL denominata `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Si noti l&#39;uso di `data-sly-test`, HTL `test` block è interessante in quanto entrambi imposta una variabile HTL AND rende/non esegue il rendering dell’elemento HTML su cui si trova, in base a se il risultato dell’espressione HTL è veritiero o meno. Se &quot;truthy&quot;, viene eseguito il rendering dell’elemento HTML, altrimenti non viene eseguito il rendering.

   Questa variabile HTL `hasContent` può ora essere riutilizzato per mostrare/nascondere il segnaposto in modo condizionato.

1. Aggiorna la chiamata condizionale a `placeholderTemplate` nella parte inferiore del file con le seguenti caratteristiche:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Visualizzare l’immagine utilizzando i componenti core {#using-the-core-components-image}

Lo script HTL per `byline.html` è quasi completo e manca solo l&#39;immagine.

Poiché utilizziamo `sling:resourceSuperType` Il componente Immagine dei componenti core per fornire l’authoring dell’immagine, possiamo anche utilizzare il componente Immagine del componente core per eseguire il rendering dell’immagine.

A questo scopo, è necessario includere la risorsa per riga corrente, ma forzare il tipo di risorsa del componente Immagine dei componenti core, utilizzando il tipo di risorsa `core/wcm/components/image/v2/image`. Questo è un modello potente per il riutilizzo dei componenti. Per questo, HTL `data-sly-resource` viene utilizzato block.

1. Sostituisci il `div` con una classe di `cmp-byline__image` con le seguenti caratteristiche:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Questo `data-sly-resource`, includeva la risorsa corrente tramite il percorso relativo `'.'`, e forza l’inclusione della risorsa corrente (o della risorsa del contenuto della riga precedente) con il tipo di risorsa di `core/wcm/components/image/v2/image`.

   Il tipo di risorsa del componente core viene utilizzato direttamente e non tramite un proxy, perché si tratta di un utilizzo in-script e non viene mai persistito nel contenuto.

2. Completato `byline.html` di seguito:

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Distribuisci la base di codice in un&#39;istanza AEM locale. Poiché sono state apportate modifiche a `core` e `ui.apps` entrambi i moduli devono essere distribuiti.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Se distribuisci a AEM 6.5/6.4, richiama il `classic` profilo:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Puoi anche creare l’intero progetto dalla radice utilizzando il profilo Maven `autoInstallSinglePackage` ma questo potrebbe sovrascrivere le modifiche al contenuto della pagina. Questo perché il `ui.content/src/main/content/META-INF/vault/filter.xml` è stato modificato per consentire al codice iniziale dell’esercitazione di sovrascrivere in modo pulito il contenuto AEM esistente. In uno scenario reale non sarà un problema.

### Revisione del componente Byline senza stile {#reviewing-the-unstyled-byline-component}

1. Dopo aver distribuito l’aggiornamento, passa alla sezione [Guida completa a LA Skatepark ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) o ovunque sia stato aggiunto il componente Byline prima nel capitolo.

1. La **immagine**, **name** e **professioni** ora viene visualizzato e abbiamo un componente Byline non formattato ma funzionante.

   ![componente non formattato per riga](assets/custom-component/unstyled.png)

### Verifica della registrazione del modello Sling {#reviewing-the-sling-model-registration}

La [Visualizzazione dello stato dei modelli Sling della console Web AEM](http://localhost:4502/system/console/status-slingmodels) visualizza tutti i modelli Sling registrati in AEM. Il modello Byline Sling può essere convalidato come installato e riconosciuto esaminando questo elenco.

Se la **BylineImpl** non viene visualizzato in questo elenco, quindi è probabile che si sia verificato un problema con le annotazioni del modello Sling oppure che il modello Sling non sia stato aggiunto al pacchetto Sling Models registrato (`com.adobe.aem.guides.wknd.core.models`) nel progetto principale.

![Modello Sling Byline registrato](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## Stili di battitura {#byline-styles}

Il componente Byline deve essere formattato per essere allineato al design creativo del componente Byline. Ciò sarà ottenuto utilizzando SCSS, che AEM il supporto per tramite il **ui.frontend** Sottoprogetto Maven.

### Aggiungi uno stile predefinito

Aggiungi gli stili predefiniti per il componente Byline.

1. Torna all’IDE e al **ui.frontend** progetto `/src/main/webpack/components`:
1. Crea un nuovo file denominato `_byline.scss`.

   ![explorer di progetto byline](assets/custom-component/byline-style-project-explorer.png)

1. Aggiungi il CSS delle implementazioni Byline (scritto come SCSS) nel `_byline.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```
1. Apri un terminale e naviga nel `ui.frontend` modulo .
1. Avvia la `watch` elabora con il seguente comando npm:

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Torna al browser e passa alla [L&#39;articolo SkateParks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Dovresti visualizzare gli stili aggiornati del componente.

   ![componente per riga finale](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Potrebbe essere necessario cancellare la cache del browser per garantire che non venga servito CSS obsoleti e aggiornare la pagina con il componente Byline per ottenere lo stile completo.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato un componente personalizzato da zero utilizzando Adobe Experience Manager!

### Passaggi successivi {#next-steps}

Continua a scoprire AEM sviluppo di componenti esplorando come scrivere test JUnit per il codice Java Byline per garantire che tutto sia sviluppato correttamente e che la logica di business implementata sia corretta e completa.

* [Test di unità di scrittura o componenti AEM](unit-testing.md)

Visualizza il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) o rivedi e distribuisci il codice localmente in sul blocco Git `tutorial/custom-component-solution`.

1. Clona il [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) archivio.
1. Consulta la sezione `tutorial/custom-component-solution` filiale
