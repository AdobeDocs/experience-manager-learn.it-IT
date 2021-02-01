---
title: Guida introduttiva  AEM Sites - Nozioni di base sui componenti
description: Scopri la tecnologia di base di un componente Adobe Experience Manager (AEM) Sites tramite un semplice esempio "HelloWorld". Vengono trattati argomenti quali HTL, Sling Models, librerie lato client e finestre di dialogo per l’authoring.
sub-product: sites
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1066'
ht-degree: 1%

---


# Nozioni di base sui componenti {#component-basics}

Questo capitolo illustra la tecnologia di base di un componente di siti Adobe Experience Manager (AEM) attraverso un semplice esempio `HelloWorld`. Verranno apportate piccole modifiche a un componente esistente, con argomenti quali authoring, HTL, Sling Models e librerie lato client.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

## Obiettivo {#objective}

1. Scoprite il ruolo dei modelli HTL e Sling Models per eseguire il rendering HTML in modo dinamico.
1. Comprendere in che modo le finestre di dialogo vengono utilizzate per facilitare la creazione di contenuto.
1. Scoprite le nozioni di base delle librerie lato client per includere CSS e JavaScript per il supporto di un componente.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo verranno apportate diverse modifiche a un semplice componente `HelloWorld`. Durante il processo di aggiornamento del componente `HelloWorld`, verranno fornite informazioni sulle aree chiave dello sviluppo AEM componente.

## Progetto iniziale capitolo {#starter-project}

Questo capitolo si basa su un progetto generico generato da [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Guardate il video seguente e controllate i [prerequisiti](#prerequisites) per iniziare!

>[!NOTE]
>
> Se il capitolo precedente è stato completato correttamente, è possibile riutilizzare il progetto e ignorare i passaggi per il check-out del progetto iniziale.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Aprite un nuovo terminale della riga di comando ed eseguite le seguenti operazioni.

1. In una directory vuota, clonate il repository [aem-guide-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Facoltativamente, puoi continuare a utilizzare il progetto generato nel capitolo precedente, [Configurazione progetto](./project-setup.md).

1. Individuate la cartella `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Create e distribuite il progetto in un&#39;istanza locale di AEM con il seguente comando:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzate AEM 6.5 o 6.4, aggiungete il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importa il progetto nell&#39;IDE desiderato seguendo le istruzioni per impostare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

## Authoring dei componenti {#component-authoring}

I componenti possono essere considerati come piccoli elementi di base modulari di una pagina Web. Per riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita mediante la finestra di dialogo di authoring. In seguito verrà creato un componente semplice e verrà analizzato il modo in cui i valori contenuti nella finestra di dialogo vengono AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Create una nuova pagina denominata **Component Basics** sotto **WKND Site** `>` **US** `>` **en**.
1. Aggiungete il **componente Hello World** alla pagina appena creata.
1. Aprire la finestra di dialogo del componente e immettere del testo. Salvare le modifiche per visualizzare il messaggio sulla pagina.
1. Passate alla modalità sviluppatore e visualizzate il Percorso contenuto in CRXDE-Lite, quindi controllate le proprietà dell’istanza del componente.
1. Utilizzare CRXDE-Lite per visualizzare lo script `cq:dialog` e `helloworld.html` situato in `/apps/wknd/components/content/helloworld`.

## HTL (HTML Template Language) e finestre di dialogo {#htl-dialogs}

HTML Template Language o **[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)** è un linguaggio di modellazione lato server leggero utilizzato dai componenti AEM per il rendering del contenuto.

**Le** finestre di dialogo definiscono le configurazioni disponibili per un componente.

Successivamente verrà aggiornato lo script `HelloWorld` HTL per visualizzare un messaggio di saluto aggiuntivo prima del messaggio di testo.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Passate all&#39;IDE e aprite il progetto nel modulo `ui.apps`.
1. Aprite il file `helloworld.html` e apportate una modifica al codice HTML.
1. Utilizzate gli strumenti IDE per sincronizzare la modifica del file con l&#39;istanza AEM locale.
1. Tornate al browser e osservate che il rendering del componente è cambiato.
1. Aprire il file `.content.xml` che definisce la finestra di dialogo per il componente `HelloWorld` in:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Aggiornare la finestra di dialogo per aggiungere un campo di testo aggiuntivo denominato **Title** con un nome di `./title`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. Apri di nuovo il file `helloworld.html`, che rappresenta lo script HTL principale responsabile del rendering del componente `HelloWorld`, situato in:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Aggiornate `helloworld.html` per eseguire il rendering del valore del campo di testo **Greeting** come parte di un tag `H1`:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Distribuite le modifiche in un&#39;istanza locale di AEM utilizzando il plug-in sviluppatore o utilizzando le vostre abilità Paradiso.

## Modelli Sling {#sling-models}

I modelli Sling sono modelli Java &quot;POJO&quot; basati su annotazioni (Plain Old Java Objects) che semplificano la mappatura dei dati dalle variabili JCR a Java e forniscono una serie di altre caratteristiche quando si sviluppano nel contesto di AEM.

Verranno quindi eseguiti alcuni aggiornamenti al modello `HelloWorldModel` Sling per applicare una qualche logica di business ai valori memorizzati nel JCR prima di inviarli alla pagina.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Aprire il file `HelloWorldModel.java`, ovvero il modello Sling utilizzato con il componente `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Aggiungete le seguenti istruzioni di importazione:

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Aggiornate l&#39;annotazione `@Model` per utilizzare un `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Aggiungere le righe seguenti alla classe `HelloWorldModel` per mappare i valori delle proprietà JCR del componente `title` e `text` alle variabili Java:

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       protected String title;
   
       @ValueMapValue
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. Aggiungete il seguente metodo `getTitle()` alla classe `HelloWorldModel`, che restituisce il valore della proprietà denominata `title`. Questo metodo aggiunge la logica aggiuntiva per restituire un valore String &quot;Default Value here!&quot; se la proprietà `title` è null o vuota:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Aggiungete il seguente metodo `getText()` alla classe `HelloWorldModel`, che restituisce il valore della proprietà denominata `text`. Questo metodo trasforma la stringa in tutti i caratteri maiuscoli.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Creare e distribuire il bundle dal modulo `core`:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Se si utilizza AEM 6.4/6.5 utilizzare `mvn clean install -PautoInstallBundle -Pclassic`

1. Aggiornare il file `helloworld.html` in `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` per utilizzare i metodi appena creati del modello `HelloWorld`:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld"
   data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Distribuite le modifiche in un&#39;istanza locale di AEM utilizzando il plug-in per sviluppatori Eclipse o utilizzando le vostre abilità Maven.

## Librerie lato client {#client-side-libraries}

Client-Side Libraries, clientlibs in breve, fornisce un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un&#39;implementazione  AEM Sites. Le librerie lato client sono il metodo standard per includere CSS e JavaScript in una pagina di AEM.

A questo punto, includeremo alcuni stili CSS per il componente `HelloWorld` per comprendere le nozioni di base delle librerie lato client.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. in `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` creare una nuova cartella denominata `clientlib-helloworld`.
1. Crea una struttura di cartelle e file come segue sotto `clientlibs`

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. Compilare `helloworld.css` con i seguenti elementi:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
       color: red;
   }
   ```

1. Compilare `helloworld/clientlibs/css.txt` con i seguenti elementi:

   ```plain
   #base=css
   helloworld.css
   ```

1. Compilare `helloworld/clientlibs/js/helloworld.js` con i seguenti elementi:

   ```js
   console.log("Hello World from Javascript!");
   ```

1. Compilare `helloworld/clientlibs/js.txt` con i seguenti elementi:

   ```plain
   #base=js
   helloworld.js
   ```

1. Aggiornate il file `clientlib-helloworld/.conten.xml` in modo da includere le seguenti proprietà:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Aggiornare il file `clientlibs/clientlib-base/.content.xml` in **incorporare** la categoria `wknd.helloworld`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Distribuite le modifiche in un&#39;istanza locale di AEM utilizzando il plug-in sviluppatore o utilizzando le vostre abilità Paradiso.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena imparato le basi dello sviluppo di componenti in Adobe Experience Manager!

### Passaggi successivi {#next-steps}

Acquisisci familiarità con le pagine e i modelli Adobe Experience Manager nel capitolo successivo [Pagine e modelli](pages-templates.md). Scopri in che modo i componenti core sono associati al progetto e impara le configurazioni di criteri avanzate dei modelli modificabili per creare un modello di pagina articolo ben strutturato.

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente sul ramo Git `tutorial/component-basics-solution`.

