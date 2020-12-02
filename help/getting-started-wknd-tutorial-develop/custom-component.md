---
title: Componente personalizzato
description: Copre la creazione finale di un componente byline personalizzato che visualizza il contenuto creato. Include lo sviluppo di un modello Sling per racchiudere la logica aziendale per compilare il componente byline e il corrispondente HTL per eseguire il rendering del componente.
sub-product: sites
feature: sling-models
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '4011'
ht-degree: 0%

---


# Componente personalizzato {#custom-component}

Questa esercitazione descrive la creazione end-to-end di un componente personalizzato AEM Byline che visualizza il contenuto creato in una finestra di dialogo ed esplora lo sviluppo di un modello Sling per racchiudere logica aziendale che popola l’HTL del componente.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

### Progetto iniziale

>[!NOTE]
>
> Se avete seguito le istruzioni nelle parti precedenti dell&#39;esercitazione, noterete che il progetto iniziale per questo capitolo accelera l&#39;implementazione. Include alcuni altri modelli e molto altro contenuto. Inoltre, è possibile esplorare liberamente i nuovi contenuti e altre aree dell&#39;implementazione, al di fuori dello sviluppo di componenti personalizzati.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Duplicare il repository [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `custom-component/start`

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout custom-component/start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/custom-component/solution) o estrarre il codice localmente passando al ramo `custom-component/solution`.

## Obiettivo

1. Come creare un componente AEM personalizzato
1. Scopri come incorporare la logica di business con Sling Models
1. Come utilizzare un modello Sling da uno script HTL

## Cosa verrà creato {#byline-component}

>[!VIDEO](https://video.tv.adobe.com/v/30181/?quality=12&learn=on)

In questa parte dell&#39;esercitazione WKND, viene creato un componente autore che verrà utilizzato per visualizzare le informazioni create sul collaboratore di un articolo.

![esempio componente byline](./assets/custom-component/byline-design.png)

*Byline component video design fornito dal team di progettazione WKND*

L&#39;implementazione del componente Byline include una finestra di dialogo che raccoglie il contenuto del byline e un modello Sling personalizzato che recupera i valori del byline:

* Nome
* Immagine
* Occupazioni

per la visualizzazione da parte di uno script HTL, che esegue il rendering dell&#39;HTML visualizzato dal browser alla fine.

![scomposizione per riga](./assets/custom-component/byline-decomposition.png)

*Scomposizione componente autore*

## Crea componente Byline {#create-byline-component}

Innanzitutto, creare la struttura del nodo del componente autore e definire una finestra di dialogo. Rappresenta il componente in AEM e definisce implicitamente il tipo di risorsa del componente in base alla sua posizione nel JCR.

La finestra di dialogo presenta l’interfaccia con la quale gli autori dei contenuti possono fornire informazioni. Per questa implementazione, il componente **Image** del componente core di AEM WCM verrà sfruttato per gestire l&#39;authoring e il rendering dell&#39;immagine del componente Byline, in modo che venga impostato come `sling:resourceSuperType` del componente.

### Crea nodo componente {#create-component-node}

1. Nel modulo **ui.apps**, andate a `/apps/wknd/components/content` e create un nuovo nodo denominato **byline** di tipo `cq:Component`.

   ![finestra di dialogo per la creazione del nodo](./assets/custom-component/byline-node-creation.png)

1. Aggiungete le seguenti proprietà al nodo `cq:Component` del componente Byline.

   ```plain
   jcr:title = Byline
   jcr:description = Displays a contributor's byline.
   componentGroup = WKND.Content
   sling:resourceSuperType =  core/wcm/components/image/v2/image
   ```

   ![Proprietà del componente autore](./assets/custom-component/byline-component-properties.png)

   Il che determina il seguente codice XML `.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND.Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

### Creare lo script HTL {#create-the-htl-script}

1. Sotto il nodo `byline`, aggiungete un nuovo file `byline.html`, responsabile della presentazione HTML del componente. È importante denominare il file come il nodo `cq:Component`, in quanto diventa lo script predefinito che Sling utilizzerà per eseguire il rendering di questo tipo di risorsa.

1. Aggiungete il codice seguente alla cartella `byline.html`.

   ```xml
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` viene  [rivisitata in seguito](#byline-htl), una volta creato il modello Sling. Lo stato corrente del file HTL consente al componente di essere visualizzato in uno stato vuoto, nell’Editor pagina di AEM Sites, quando viene trascinato e rilasciato sulla pagina.

### Creare la definizione di finestra di dialogo {#create-the-dialog-definition}

Quindi, definire una finestra di dialogo per il componente Autore con i campi seguenti:

* **Nome**: un campo di testo con il nome del collaboratore.
* **Immagine**: un riferimento alla biografia del collaboratore.
* **Occupazioni**: un elenco di occupazioni attribuite al collaboratore. Le occupazioni devono essere ordinate in ordine alfabetico in ordine crescente (da a a z).

1. Sotto il nodo del componente `byline` create un nuovo nodo denominato `cq:dialog` di tipo `nt:unstructured`.
1. Aggiornate il file `cq:dialog` con il seguente XML. È più semplice aprire il `.content.xml` e copiare/incollare il seguente XML in esso.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
           xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
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

   Queste definizioni di nodo utilizzano [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) per controllare quali schede di dialogo vengono ereditate dal componente `sling:resourceSuperType`, in questo caso il componente Immagine **Componenti di base**.

   ![finestra di dialogo completata per autore](./assets/custom-component/byline-dialog-created.png)

### Crea la finestra di dialogo Criterio {#create-the-policy-dialog}

Seguendo lo stesso approccio utilizzato per la creazione della finestra di dialogo, creare una finestra di dialogo Criteri (precedentemente nota come finestra di dialogo Progettazione) per nascondere i campi indesiderati nella configurazione Criteri ereditata dal componente Immagine dei componenti core.

1. Sotto il nodo `byline` `cq:Component`, creare un nuovo nodo denominato `cq:design_dialog` di tipo `nt:unstructured`.
1. Aggiornate il file `cq:design_dialog` con il seguente XML. È più semplice aprire il `.content.xml` e copiare/incollare il codice XML sottostante.

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

   La base per la precedente finestra di dialogo **Criteri** XML è stata ottenuta dal componente [Immagine componenti core](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Come nella configurazione della finestra di dialogo, [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) viene utilizzato per nascondere i campi irrilevanti che vengono altrimenti ereditati dalla `sling:resourceSuperType`, come indicato dalle definizioni dei nodi con la proprietà `sling:hideResource="{Boolean}true"`.

### Distribuzione del codice {#deploy-the-code}

1. Distribuisci la base di codice aggiornata in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallPackage
   ```

### Aggiungere il componente a una pagina {#add-the-component-to-a-page}

Per semplificare e concentrarsi sullo sviluppo AEM componente, verrà aggiunto il componente Autore nello stato corrente a una pagina Articolo per verificare che la definizione del nodo `cq:Component` sia distribuita e corretta, AEM riconosce la nuova definizione del componente e la finestra di dialogo del componente funziona per la creazione.

Poiché abbiamo [aggiunto il componente Byline al **WKND.Content** Gruppo di componenti](#create-component-node), tramite la proprietà `/apps/wknd/components/content/byline@componentGroup=WKND.Content`, è automaticamente disponibile per qualsiasi **Contenitore di layout** il cui **Policy** consente il **WKND.Content** gruppo di componenti , che è il contenitore di layout della pagina dell&#39;articolo.

#### Trascinare il componente sulla pagina {#drag-and-drop-the-component-onto-the-page}

1. **Modificate** la pagina dell&#39;articolo in  **AEM > Siti > WKND Site > Language Master > English > Magazine > Ultimate Guide to LA Skateparks**.
1. Dalla barra laterale sinistra, trascinate un **componente autore** sul **fondo** del Contenitore di layout della pagina dell&#39;articolo aperta.

   ![aggiunta di un componente per riga alla pagina](assets/custom-component/add-to-page.png)

#### Creazione del componente {#author-the-component}

AEM autori configurano e creano i componenti tramite le finestre di dialogo. A questo punto nello sviluppo del componente Byline le finestre di dialogo sono incluse per la raccolta dei dati, tuttavia la logica per il rendering del contenuto creato non è ancora stata aggiunta.

1. Assicurarsi che la barra laterale sinistra sia aperta **e visibile, e che sia selezionato** Asset Finder **.**

   ![apri risorsa](assets/custom-component/open-asset-finder.png)

1. Selezionare il segnaposto del componente **Autore**, che a sua volta visualizza la barra delle azioni e toccare l&#39;icona **chiave inglese** per aprire la finestra di dialogo.

   ![barra delle azioni del componente](assets/custom-component/action-bar.png)

1. Con la finestra di dialogo aperta e la prima scheda (Risorsa) attiva, aprite la barra laterale sinistra, quindi trascinate un’immagine nella zona di rilascio Immagine da Content Finder. Cercate &quot;stack&quot; per trovare Stacey Roswells bio picture fornito nel pacchetto ui.content WKND.

   **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**

   ![aggiungi immagine alla finestra di dialogo](assets/custom-component/add-image.png)

1. Dopo aver aggiunto un&#39;immagine, fare clic sulla scheda **Proprietà** per inserire il **Nome** e **Occupazioni**.

   Quando si inseriscono le occupazioni, inserirle nell&#39;ordine **alfabetico inverso** in modo che la logica di business dell&#39;ordine alfabetizzazione che verrà implementata nel modello Sling sia facilmente visibile.

   Toccate il pulsante **Fine** in basso a destra per salvare le modifiche.

   ![popolamento delle proprietà del componente byline](assets/custom-component/add-properties.png)

1. Dopo aver salvato la finestra di dialogo, andate a [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline) e controllate in che modo il contenuto del componente viene memorizzato nel nodo del contenuto del componente secondario sotto la pagina AEM.

   Individuare il nodo di contenuto del componente Byline sotto il nodo `jcr:content/root/responsivegrid/responsivegrid`, ovvero `/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline`.

   Notate che i nomi delle proprietà `name`, `occupations` e `fileReference` sono memorizzati nel nodo **byline**.

   Inoltre, notate che la `sling:resourceType` del nodo è impostata su `wknd/components/content/byline`, che è ciò che vincola questo nodo di contenuto all&#39;implementazione del componente Byline.

   ![proprietà del byline in CRXDE](assets/custom-component/byline-properties-crxde.png)

   */content/wknd/language-masters/it/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline*

## Crea modello Sling byline {#create-sling-model}

Verrà quindi creato un modello Sling che fungerà da modello di dati e da struttura logica di business per il componente Byline.

I modelli Sling sono modelli Java &quot;POJO&quot; basati su annotazioni (Plain Old Java Objects) che semplificano la mappatura dei dati dalle variabili JCR a Java e forniscono una serie di altre caratteristiche quando si sviluppano nel contesto di AEM.

### Rivedere le dipendenze del cielo {#maven-dependency}

Il modello Byline Sling si basa su diverse API Java fornite da AEM. Queste API sono rese disponibili tramite `dependencies` elencato nel file POM del modulo `core`.

1. Aprire il file `pom.xml` sotto `<src>/aem-guides-wknd/core/pom.xml`.
1. Trovare la dipendenza per la `uber-jar` nella sezione delle dipendenze del file pom:

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   [uber-jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contiene tutte le API Java pubbliche esposte da AEM. Notate che una versione non è specificata nel file `core/pom.xml`. La versione viene invece mantenuta nel contenitore del reattore principale, situato alla radice del progetto `aem-guides-wknd/pom.xml`.

1. Trovare la dipendenza per `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Si tratta di tutte le API Java pubbliche esposte da AEM componenti core. AEM Componenti di base è un progetto mantenuto al di fuori di AEM e ha quindi un ciclo di rilascio separato. Per questo motivo si tratta di una dipendenza che deve essere inclusa separatamente ed è **non** inclusa con l&#39;uber-jar.

   Come l&#39;uber-jar, la versione di questa dipendenza viene mantenuta nel file pom del reattore principale che si trova in `aem-guides-wknd/pom.xml`.

   Più avanti in questa esercitazione utilizzeremo la classe Immagine componente principale per visualizzare l’immagine nel componente Autore. È necessario avere la dipendenza del componente core per creare e compilare il nostro modello Sling.

### Interfaccia autore {#byline-interface}

Create un&#39;interfaccia Java pubblica per il nome autore. `Byline.java` definisce i metodi pubblici necessari per eseguire lo script  `byline.html` HTL.

1. Nel modulo `aem-guides-wknd.core` sotto `src/main/java,` create una nuova interfaccia Java denominata `Byline.java` facendo clic con il pulsante destro del mouse sul `com.adobe.aem.guides.wknd.core.models` **pacchetto > Nuovo > Interfaccia**. Immettere **Nome autore** come nome dell&#39;interfaccia e fare clic su Fine.

   ![crea interfaccia per riga](assets/custom-component/create-byline-interface.png)

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

   I primi due metodi espongono i valori per le **name** e **occupazioni** del componente Byline.

   Il metodo `isEmpty()` viene utilizzato per determinare se il componente ha del contenuto da riprodurre o se è in attesa di essere configurato.

   Notate che non esiste un metodo per l’immagine; [Verranno fornite informazioni sul motivo per cui si tratterà più tardi](#tackling-the-image-problem).

### Implementazione autore {#byline-implementation}

`BylineImpl.java` è l&#39;implementazione del modello Sling che implementa l&#39; `Byline.java` interfaccia definita in precedenza. Il codice completo di `BylineImpl.java` si trova nella parte inferiore di questa sezione.

1. Nel modulo `core` sotto `src/main/java`, create un nuovo file di classe denominato **BylineImpl.java** facendo clic con il pulsante destro del mouse sul pacchetto `com.adobe.aem.guides.wknd.core.models.impl` e selezionando **Nuovo > Classe**.

   Per il nome, immettere **BylineImpl**. Aggiungete l&#39; **Interfaccia autore** come interfaccia di implementazione.

   ![creazione dell&#39;implementazione byline](assets/custom-component/create-byline-impl.png)

1. Apri `BylineImpl.java`. Viene compilato automaticamente con tutti i metodi definiti nell&#39;interfaccia `Byline.java`. Aggiungete le annotazioni del modello Sling aggiornando `BylineImpl.java` con le seguenti annotazioni a livello di classe. Questa `@Model(..)`annotazione è ciò che trasforma la classe in un modello Sling.

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
       ...
   }
   ```

   Esaminiamo questa annotazione e i relativi parametri:

   * L&#39;annotazione `@Model` registra BylineImpl come modello Sling quando viene distribuita per AEM.
   * Il parametro `adaptables` specifica che questo modello può essere adattato dalla richiesta.
   * Il parametro `adapters` consente di registrare la classe di implementazione nell&#39;interfaccia Byline. Questo consente allo script HTL di chiamare il modello Sling tramite l&#39;interfaccia (invece dell&#39;impl direttamente). [Maggiori dettagli sulle schede di rete sono disponibili qui](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * Il `resourceType` indica il tipo di risorsa del componente Byline (creato in precedenza) e aiuta a risolvere il modello corretto in presenza di più implementazioni. [Ulteriori dettagli sull&#39;associazione di una classe modello a un tipo di risorsa sono disponibili qui](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementazione dei metodi del modello Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

Il primo metodo che utilizzeremo è `getName()` che restituisce semplicemente il valore memorizzato nel nodo di contenuto JCR del byline sotto la proprietà `name`.

A questo scopo, l&#39;annotazione `@ValueMapValue` Modello Sling viene utilizzata per inserire il valore in un campo Java utilizzando il ValueMap della risorsa Request.

```java
...
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
...
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

Il metodo successivo da implementare è `getOccupations()`. Questo metodo raccoglie tutte le occupazioni memorizzate nella proprietà JCR `occupations` e restituisce un insieme ordinato (in ordine alfabetico) di esse.

Utilizzando la stessa tecnica esplorata in `getName()` il valore della proprietà può essere inserito nel campo Modello Sling.

Una volta che i valori delle proprietà JCR sono disponibili nel modello Sling tramite il campo Java iniettato `occupations`, la logica aziendale di ordinamento può essere applicata nel metodo `getOccupations()`.

```java
import java.util.ArrayList;
import java.util.Collections;
...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
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

L&#39;ultimo metodo pubblico è `isEmpty()` che determina quando il componente deve considerarsi &quot;sufficientemente creato&quot; per il rendering.

Per questo componente, i requisiti aziendali prevedono che tutti e tre i campi, nome, immagine e occupazioni debbano essere compilati *prima di poter eseguire il rendering del componente.*

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

#### Gestione del &quot;problema immagine&quot; {#tackling-the-image-problem}

Controllare il nome e le condizioni di occupazione sono banali (e l&#39;Apache Commons Lang3 fornisce la sempre pratica classe [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html)), tuttavia, non è chiaro come la **presenza dell&#39;immagine** possa essere convalidata, dal momento che il componente Immagine Componenti di base viene utilizzato per la superficie dell&#39;immagine.

Esistono due modi per affrontare il problema:

1. Verificate che la proprietà `fileReference` JCR corrisponda a una risorsa.
1. Converti questa risorsa in un modello Sling di immagini per componenti core e assicurati che il metodo `getSrc()` non sia vuoto.

   Sceglieremo l&#39;approccio **secondo**. Il primo approccio è probabilmente sufficiente, ma in questa esercitazione quest&#39;ultimo sarà utilizzato per consentirci di esplorare altre caratteristiche di Sling Models.

1. Create un metodo privato per ottenere l’immagine. Questo metodo viene lasciato privato perché non è necessario esporre l’oggetto Immagine nello stesso HTL, e viene utilizzato solo per l’unità `isEmpty().`

   Il seguente metodo privato per `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Come già detto, ci sono altri due approcci per ottenere il **modello Sling immagine**:

   Il primo utilizza l&#39;annotazione `@Self` per adattare automaticamente la richiesta corrente alla `Image.class` del componente principale

   ```java
   @Self
   private Image image;
   ```

   Il secondo utilizza il servizio [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi, che è un servizio molto pratico, e ci aiuta a creare Sling Models di altri tipi nel codice Java.

   Sceglieremo il secondo approccio.

   >[!NOTE]
   >
   >In un&#39;implementazione realistica, l&#39;approccio &quot;One&quot;, che utilizza `@Self` è la soluzione più semplice ed elegante, è la soluzione ideale. In questa esercitazione utilizzeremo il secondo approccio, in quanto ci richiede di esplorare più sfaccettature di Sling Models che sono estremamente utili è componenti più complessi!

   Poiché i modelli Sling sono Java POJO e non OSGi Services, le note di iniezione OSGi `@Reference` **non possono essere utilizzate**, i modelli Sling forniscono invece un&#39;annotazione speciale **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** che fornisce funzionalità simili.

1. Aggiornate `BylineImpl.java` per includere l&#39;annotazione `OSGiService` per inserire `ModelFactory`:

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

   Con `ModelFactory` disponibile, è possibile creare un modello Sling per immagini di componente principale utilizzando:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Tuttavia, questo metodo richiede sia una richiesta che una risorsa, non ancora disponibili nel modello Sling. Per ottenerli, vengono utilizzate più annotazioni modello Sling!

   Per ottenere la richiesta corrente, l&#39;annotazione **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** può essere utilizzata per inserire la `adaptable` (definita in `@Model(..)` come `SlingHttpServletRequest.class` in un campo di classe Java).

1. Aggiungete l&#39;annotazione **@Self** per ottenere la richiesta **SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Ricordate che l&#39;utilizzo di `@Self Image image` per inserire il modello Sling immagine del componente principale era un&#39;opzione sopra - l&#39; `@Self` annotazione tenta di inserire l&#39;oggetto adattabile (nel nostro caso SlingHttpServletRequest) e di adattarsi al tipo di campo di annotazione. Poiché il modello Sling per immagini del componente principale è adattabile dagli oggetti SlingHttpServletRequest, questo avrebbe funzionato ed è meno codice rispetto al nostro approccio più esplorativo.

   Ora abbiamo inserito le variabili necessarie per creare un&#39;istanza del nostro modello Immagine tramite l&#39;API ModelFactory. Per ottenere questo oggetto dopo la creazione dell&#39;istanza Sling Model, verrà utilizzata l&#39;annotazione **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)**.

   `@PostConstruct` è incredibilmente utile e agisce in una capacità simile come costruttore, tuttavia, viene richiamato dopo che la classe è stata creata e tutti i campi Java annotati vengono inseriti. Mentre altre annotazioni modello Sling annotano i campi di classe Java (variabili), `@PostConstruct` annota un metodo di parametro void, zero, in genere denominato `init()` (ma può essere denominato qualsiasi cosa).

1. Aggiungi il metodo **@PostConstruct**:

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

   I modelli Sling sono **NOT** OSGi Services, pertanto è sicuro mantenere lo stato della classe. Spesso `@PostConstruct` deriva e imposta lo stato della classe Sling Model per un uso successivo, simile a quello di un costruttore normale.

   Se il metodo `@PostConstruct` genera un&#39;eccezione, il modello Sling non crea un&#39;istanza (sarà null).

1. **getImage()** può ora essere aggiornato per restituire semplicemente l&#39;oggetto immagine.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Torniamo a `isEmpty()` e terminiamo l&#39;implementazione:

   ```java
   @Override
   public boolean isEmpty() {
       ...
       } else if (getImage() == null || StringUtils.isBlank(getImage().getSrc())) {
           // A valid image is required
           return true;
       } else {
       ...
   }
   ```

   Notare che più chiamate a `getImage()` non presentano problemi in quanto restituisce la variabile di classe `image` inizializzata e non richiama `modelFactory.getModelFromWrappedRequest(...)` che non è un costo eccessivo, ma vale la pena evitare chiamate inutilmente.

1. L&#39;aspetto finale `BylineImpl.java` dovrebbe essere:

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       @PostConstruct
       private void init() {
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
           final Image image = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (image == null || StringUtils.isBlank(image.getSrc())) {
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

Nel modulo `ui.apps`, aprire `/apps/wknd/components/content/byline/byline.html` creato nella precedente configurazione del componente AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Vediamo cosa fa finora questo script HTL:

* `placeholderTemplate` indica il segnaposto dei componenti core, visualizzato quando il componente non è completamente configurato. Viene visualizzato in  AEM Sites Page Editor come una casella con il titolo del componente, come definito sopra nella proprietà `cq:Component` `jcr:title`.

* Il `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carica il `placeholderTemplate` definito sopra e trasmette un valore booleano (attualmente codificato in `false`) nel modello del segnaposto. Se `isEmpty` è true, il modello segnaposto riproduce la casella grigia, altrimenti non esegue il rendering.

### Aggiorna HTL autore

1. Aggiornate **byline.html** con la seguente struttura HTML scheletrica:

   ```xml
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!-- Include the Core Components Image Component -->
           </div>
           <h2 class="cmp-byline__name"><!-- Include the name --></h2>
           <p class="cmp-byline__occupations"><!-- Include the occupations --></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Le classi CSS seguono la [convenzione di denominazione BEM](https://getbem.com/naming/). Anche se l&#39;uso delle convenzioni BEM non è obbligatorio, BEM è consigliato in quanto è utilizzato nelle classi CSS dei componenti core e generalmente produce regole CSS pulite e leggibili.

#### Creazione di un&#39;istanza di oggetti modello Sling in HTL {#instantiating-sling-model-objects-in-htl}

L&#39;istruzione [Use block istruzione](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) viene utilizzata per creare un&#39;istanza degli oggetti modello Sling nello script HTL e assegnarla a una variabile HTL.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utilizza l’interfaccia Byline (com.adobe.aem.guide.wknd.models.Byline) implementata da BylineImpl e adatta l’attuale SlingHttpServletRequest e il risultato viene memorizzato in una variabile HTL name by line (  `data-sly-use.<variable-name>`).

1. Aggiornare il modello Sling esterno `div` per fare riferimento al modello **Byline** tramite la relativa interfaccia pubblica:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

#### Accesso ai metodi del modello Sling {#accessing-sling-model-methods}

HTL prende in prestito da JSTL e utilizza lo stesso accorciamento dei nomi dei metodi getter Java.

Ad esempio, richiamando il metodo `getName()` del modello Byline Sling è possibile ridurre a `byline.name`, in modo simile invece di `byline.isEmpty`, è possibile abbreviarlo a `byline.empty`. Anche l&#39;utilizzo di nomi di metodo completi, `byline.getName` o `byline.isEmpty`, funziona. Notate che `()` non viene mai utilizzato per richiamare metodi in HTL (simile a JSTL).

I metodi Java che richiedono un parametro **non possono essere utilizzati** in HTL. Questo è per progettazione per mantenere la logica in HTL semplice.

1. Il nome dell’autore può essere aggiunto al componente richiamando il metodo `getName()` nel modello Sling per autore o in HTL: `${byline.name}`.

   Aggiorna il tag `h2`:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

#### Utilizzo delle opzioni di espressione HTL {#using-htl-expression-options}

[Le ](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) opzioni delle espressioni HTL agiscono da modificatori di contenuto in HTL e variano dalla formattazione delle date alla traduzione i18n. Le espressioni possono essere utilizzate anche per unire elenchi o array di valori, che è ciò che è necessario per visualizzare le occupazioni in un formato delimitato da virgole.

Le espressioni vengono aggiunte tramite l&#39;operatore `@` nell&#39;espressione HTL.

1. Per unire l’elenco di occupazioni con &quot;, &quot;, viene utilizzato il seguente codice:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

#### Visualizzazione condizionale del segnaposto {#conditionally-displaying-the-placeholder}

La maggior parte degli script HTL per AEM componenti utilizza il **paradigma segnaposto** per fornire un segnale visivo agli autori **indicando che un componente non è stato creato correttamente e non verrà visualizzato in AEM Publish**. La convenzione per guidare questa decisione è di implementare un metodo sul modello Sling di supporto del componente, nel nostro caso: `Byline.isEmpty()`.

`isEmpty()` viene richiamato sul modello Sling autore e il risultato (o meglio il suo negativo, tramite l&#39; `!` operatore) viene salvato in una variabile HTL denominata  `hasContent`:

1. Aggiornate la variabile esterna `div` per salvare una variabile HTL denominata `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Notate l&#39;utilizzo di `data-sly-test`, il blocco HTL `test` è interessante in quanto entrambi imposta una variabile HTL AND renderizza/non esegue il rendering dell&#39;elemento HTML su cui si trova, in base a se il risultato dell&#39;espressione HTL è veritiero o meno. Se &quot;true&quot;, l&#39;elemento HTML viene riprodotto, altrimenti non viene eseguito il rendering.

   È ora possibile riutilizzare la variabile HTL `hasContent` per mostrare o nascondere il segnaposto in modo condizionale.

1. Aggiorna la chiamata condizionale alla `placeholderTemplate` in fondo al file con la seguente sintassi:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

#### Visualizzare l&#39;immagine utilizzando i componenti core {#using-the-core-components-image}

Lo script HTL per `byline.html` è quasi completo e manca solo l&#39;immagine.

```html
<!--/* current progress of byline.html */-->
<div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!byline.empty}"
     class="cmp-byline">
    <div class="cmp-byline__image">
        <!-- Include the Core Components Image component -->
    </div>
    <h2 class="cmp-byline__name">${byline.name}</h2>
    <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
```

Poiché il componente Immagine componenti core viene utilizzato `sling:resourceSuperType` per creare l’immagine, è possibile utilizzare anche il componente Immagine componente core per eseguire il rendering dell’immagine!

A tal fine, è necessario includere la risorsa corrente per riga, ma forzare il tipo di risorsa del componente Immagine componenti core, utilizzando il tipo di risorsa `core/wcm/components/image/v2/image`. Si tratta di un pattern potente per il riutilizzo dei componenti. Per questo, viene utilizzato il blocco HTL `data-sly-resource`.

1. Sostituite l&#39;elemento `div` con una classe di `cmp-byline__image` con quanto segue:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Questo `data-sly-resource` include la risorsa corrente tramite il percorso relativo `'.'` e forza l&#39;inclusione della risorsa corrente (o della risorsa contenuto del nome) con il tipo di risorsa `core/wcm/components/image/v2/image`.

   Il tipo di risorsa del componente di base viene utilizzato direttamente e non tramite un proxy, perché è un utilizzo in-script e non è mai persistente nel nostro contenuto.

2. Completato `byline.html` di seguito:

   ```html
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
            data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
           <h2 class="cmp-byline__name">${byline.name}</h2>
           <p class="cmp-byline__occupations">${byline.occupations @ join=','}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Distribuire la base di codice in un&#39;istanza AEM locale. Poiché sono state apportate modifiche importanti ai file POM, eseguire una build Maven completa dalla directory principale del progetto.

   >[!WARNING]
   >
   > Il progetto WKND è configurato in modo tale che `ui.content` sovrascriverà tutte le modifiche nel JCR. Pertanto, desideriamo assicurarci di distribuire solo il progetto `ui.apps` per evitare di cancellare il componente Byline aggiunto alla pagina dell&#39;articolo in precedenza.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.apps
   $ mvn -PautoInstallPackage clean install
   ...
   Package imported.
   Package installed in 338ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

#### Analisi del componente Byline senza stile {#reviewing-the-unstyled-byline-component}

1. Dopo aver distribuito l&#39;aggiornamento, andate alla pagina [Ultimate Guide to LA Skateparks ](http://localhost:4502/editor.html/content/wknd/language-masters/en/magazine/guide-la-skateparks.html) o ovunque avete aggiunto il componente Byline prima nel capitolo.

1. Viene ora visualizzata l&#39; **immagine**, **nome** e **occupazioni** e il componente Byline non è formattato ma funziona.

   ![componente non formattato](assets/custom-component/unstyled.png)

#### Revisione della registrazione del modello Sling {#reviewing-the-sling-model-registration}

La [AEM Visualizzazione stato modelli Sling della console Web ](http://localhost:4502/system/console/status-slingmodels) visualizza tutti i modelli Sling registrati in AEM. Il modello Byline Sling può essere convalidato come installato e riconosciuto revisionando questo elenco.

Se il **BylineImpl** non è visualizzato in questo elenco, è probabile che si sia verificato un problema con le annotazioni del modello Sling o che il modello Sling non sia stato aggiunto al pacchetto Sling Models registrato (com.adobe.aem.guide.wknd.core.models) nel progetto principale.

![Modello Sling autore registrato](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## Stili del byte {#byline-styles}

Il componente Byline deve essere formattato in modo da essere allineato al design creativo per il componente Byline. Ciò sarà ottenuto utilizzando SCSS, che AEM il supporto per il **ui.frontend** sottoprogetto Maven.

Dopo lo stile, il componente Byline deve adottare la seguente estetica.

![stili di battuta](./assets/custom-component/byline-design.png)

*Progettazione di componenti per autore come definito dal team creativo WKND*

### Aggiungere uno stile predefinito

Aggiungere stili predefiniti per il componente Autore. Nel progetto **ui.frontend** in `/src/main/webpack/components/content`:

1. Create una nuova cartella denominata `byline`.
1. Create una nuova cartella sotto la cartella `byline` denominata `scss`.
1. Create un nuovo file sotto la cartella `byline/scss` denominata `byline.scss`.
1. Create una nuova cartella sotto la cartella `byline/scss` denominata `styles`.
1. Create un nuovo file sotto la cartella `byline/scss/styles` denominata `default.scss`.

   ![byline project explorer](assets/custom-component/byline-style-project-explorer.png)

1. Iniziate compilando **byline.scss** per includere lo stile predefinito:

   ```scss
    /* WKND Byline styles */
   @import 'styles/default';
   ```

1. Aggiungete i CSS per le implementazioni Byline (scritti come SCSS) in `default.scss`:

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
           font-size: $font-size-large;
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

1. Aprire il file `main.scss` nel progetto **ui.frontend** in `/src/main/webpack/site` e aggiungere la riga seguente nella sezione `/* Components */`:

   ```scss
   @import '../components/content/byline/scss/byline.scss';
   ```

1. Create e compilate il modulo `ui.frontend` utilizzando NPM:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend
    $ npm run dev
   ```

1. Crea e distribuisci il progetto `ui.apps`, che includerà transitivamente il progetto `ui.frontend`, in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.apps
    $ mvn clean install -PautoInstallPackage
   ```

   >[!TIP]
   >
   >Potrebbe essere necessario cancellare la cache del browser per evitare che venga distribuito CSS obsoleto, e aggiornare la pagina con il componente Byline per ottenere lo stile completo.

## Inserirlo insieme {#putting-it-together}

Di seguito è riportato l’aspetto del componente autore e del componente Byline completamente formattato sulla pagina AEM.

![componente per riga finale](assets/custom-component/final-byline-component.png)

Guardate il video qui sotto per una rapida panoramica di ciò che è stato creato in questa esercitazione.

>[!VIDEO](https://video.tv.adobe.com/v/30174/?quality=12&learn=on)

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato un componente personalizzato da zero utilizzando Adobe Experience Manager!

### Passaggi successivi {#next-steps}

Continua a conoscere AEM sviluppo di componenti esplorando come scrivere test JUnit per il codice Java Byline per garantire che tutto sia sviluppato correttamente, e implementato business logic è corretto e completo.

* [Creazione di test di unità o componenti AEM](unit-testing.md)

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in corrispondenza del blocco Git `custom-component/solution`.

1. Duplicare il repository [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `custom-component/solution`

## Risoluzione dei problemi {#troubleshooting}

### Cartelle sorgente mancanti

Se in Eclipse non viene visualizzata la cartella di origine `src/main/java`, è possibile aggiungere le cartelle facendo clic con il pulsante destro del mouse su src e aggiungendo le cartelle per la cartella principale e per la cartella Java. Dopo aver aggiunto le cartelle, viene visualizzato il pacchetto `src/main/java`.

### Pacchetti non risolti

![risoluzione dei problemi relativi ai pacchetti non risolti](assets/custom-component/troubleshoot-unresolved-packages.png)

>[!NOTE]
>
> Se avete importazioni di pacchetti non risolte per alcune delle nuove dipendenze aggiunte al progetto principale, provate ad aggiornare il progetto aem-guide-wknd maven, che a sua volta aggiornerà tutti i sottoprogetti. A tale scopo, fare clic con il pulsante destro del mouse su **aem-guide-wknd > Maven > Update Project** (Aggiorna progetto).
