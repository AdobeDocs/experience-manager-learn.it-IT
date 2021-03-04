---
title: Guida introduttiva di AEM Sites - Nozioni di base sui componenti
description: Scopri la tecnologia di base di un componente Adobe Experience Manager (AEM) Sites tramite un semplice esempio "HelloWorld". Vengono esaminati gli argomenti relativi a HTL, modelli Sling, librerie lato client e finestre di dialogo per l’authoring.
sub-product: sites
feature: Componenti core, strumenti per sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
topic: Gestione dei contenuti, sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 1%

---


# Nozioni di base sui componenti {#component-basics}

In questo capitolo esamineremo la tecnologia di base di un componente Adobe Experience Manager (AEM) Sites tramite un semplice esempio `HelloWorld`. Verranno apportate piccole modifiche a un componente esistente, con argomenti relativi all’authoring, all’HTL, ai modelli Sling e alle librerie lato client.

## Prerequisiti {#prerequisites}

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

L&#39;IDE utilizzato nei video è [Visual Studio Code](https://code.visualstudio.com/) e il plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) .

## Obiettivo {#objective}

1. Scopri il ruolo dei modelli HTL e dei modelli Sling per eseguire il rendering dinamico dell’HTML.
1. Scopri come le finestre di dialogo vengono utilizzate per facilitare l’authoring dei contenuti.
1. Scopri le nozioni di base delle librerie lato client per includere CSS e JavaScript per supportare un componente.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo verranno apportate diverse modifiche a un componente molto semplice `HelloWorld`. Durante l’aggiornamento del componente `HelloWorld` , scopri le aree chiave per lo sviluppo di componenti AEM.

## Progetto iniziale capitolo {#starter-project}

Questo capitolo si basa su un progetto generico generato da [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Guarda il video seguente e controlla i [prerequisiti](#prerequisites) per iniziare!

>[!NOTE]
>
> Se hai completato con successo il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per il check-out del progetto iniziale.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Aprire un nuovo terminale della riga di comando ed eseguire le operazioni seguenti.

1. In una directory vuota, duplica il repository [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Facoltativamente, puoi continuare a utilizzare il progetto generato nel capitolo precedente, [Configurazione progetto](./project-setup.md).

1. Passa alla cartella `aem-guides-wknd` .

   ```shell
   $ cd aem-guides-wknd
   ```

1. Crea e implementa il progetto in un’istanza locale di AEM con il seguente comando:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importa il progetto nell&#39;IDE preferito seguendo le istruzioni per impostare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

## Authoring di componenti {#component-authoring}

I componenti possono essere considerati come piccoli blocchi modulari di una pagina web. Per riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita tramite la finestra di dialogo di authoring. Ora creeremo un componente semplice e analizzeremo il modo in cui i valori della finestra di dialogo vengono mantenuti in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Crea una nuova pagina denominata **Nozioni di base sui componenti** sotto **Sito WKND** `>` **US** `>` **en**.
1. Aggiungi il **componente Ciao a tutti** alla pagina appena creata.
1. Apri la finestra di dialogo del componente e immetti del testo. Salva le modifiche per visualizzare il messaggio sulla pagina.
1. Passa alla modalità sviluppatore e visualizza il percorso del contenuto in CRXDE-Lite ed esamina le proprietà dell&#39;istanza del componente.
1. Utilizza CRXDE-Lite per visualizzare lo script `cq:dialog` e `helloworld.html` situato in `/apps/wknd/components/content/helloworld`.

## HTL (HTML Template Language) e finestre di dialogo {#htl-dialogs}

HTML Template Language o **[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)** è un linguaggio di template lato server leggero utilizzato dai componenti AEM per il rendering del contenuto.

**** Le finestre di dialogo definiscono le configurazioni disponibili che possono essere rese per un componente.

Ora aggiorneremo lo script `HelloWorld` HTL per visualizzare un messaggio di saluto aggiuntivo prima del messaggio di testo.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Passa all’IDE e apri il progetto nel modulo `ui.apps` .
1. Apri il file `helloworld.html` e apporta una modifica al markup HTML.
1. Utilizza gli strumenti IDE come [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) per sincronizzare la modifica del file con l&#39;istanza AEM locale.
1. Torna al browser e osserva che il rendering del componente è cambiato.
1. Apri il file `.content.xml` che definisce la finestra di dialogo per il componente `HelloWorld` in:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Aggiorna la finestra di dialogo per aggiungere un campo di testo aggiuntivo denominato **Titolo** con un nome `./title`:

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

1. Riapri il file `helloworld.html`, che rappresenta lo script HTL principale responsabile del rendering del componente `HelloWorld`, situato in:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Aggiorna `helloworld.html` per eseguire il rendering del valore del campo di testo **Greeting** come parte di un tag `H1` :

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Distribuisci le modifiche in un’istanza locale di AEM utilizzando il plug-in per sviluppatori o le tue competenze Maven.

## Modelli Sling {#sling-models}

I modelli Sling sono Java &quot;POJO&quot; (Plain Old Java Objects) basati su annotazioni che facilitano la mappatura dei dati da JCR alle variabili Java e forniscono una serie di altri concetti durante lo sviluppo nel contesto di AEM.

Quindi, effettueremo alcuni aggiornamenti al modello `HelloWorldModel` Sling per applicare una certa logica di business ai valori memorizzati nel JCR prima di inviarli alla pagina.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Apri il file `HelloWorldModel.java`, che è il modello Sling utilizzato con il componente `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Aggiungi le seguenti istruzioni di importazione:

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Aggiorna l&#39;annotazione `@Model` per utilizzare un `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Aggiungi le righe seguenti alla classe `HelloWorldModel` per mappare i valori delle proprietà JCR del componente `title` e `text` alle variabili Java:

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. Aggiungi il seguente metodo `getTitle()` alla classe `HelloWorldModel` che restituisce il valore della proprietà denominata `title`. Questo metodo aggiunge la logica aggiuntiva per restituire un valore String di &quot;Valore predefinito qui!&quot; se la proprietà `title` è null o vuota:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Aggiungi il seguente metodo `getText()` alla classe `HelloWorldModel` che restituisce il valore della proprietà denominata `text`. Questo metodo trasforma la stringa in tutti i caratteri maiuscoli.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Crea e distribuisci il bundle dal modulo `core` :

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.4/6.5 utilizza `mvn clean install -PautoInstallBundle -Pclassic`

1. Aggiorna il file `helloworld.html` in `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` per utilizzare i metodi appena creati del modello `HelloWorld`:

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

1. Distribuisci le modifiche in un’istanza locale di AEM utilizzando il plugin Eclipse Developer o le tue competenze Maven.

## Librerie lato client {#client-side-libraries}

Librerie lato client, clientlibs in breve, offre un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un’implementazione di AEM Sites. Le librerie lato client sono il metodo standard per includere CSS e JavaScript in una pagina in AEM.

Ora includeremo alcuni stili CSS per il componente `HelloWorld` per comprendere le nozioni di base delle librerie lato client.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. in `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` crea una nuova cartella denominata `clientlib-helloworld`.
1. Crea una cartella e una struttura di file come la seguente sotto `clientlibs`

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. Popolare `helloworld.css` con quanto segue:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
       color: red;
   }
   ```

1. Popolare `helloworld/clientlibs/css.txt` con quanto segue:

   ```plain
   #base=css
   helloworld.css
   ```

1. Popolare `helloworld/clientlibs/js/helloworld.js` con quanto segue:

   ```js
   console.log("Hello World from Javascript!");
   ```

1. Popolare `helloworld/clientlibs/js.txt` con quanto segue:

   ```plain
   #base=js
   helloworld.js
   ```

1. Aggiorna il file `clientlib-helloworld/.conten.xml` in modo da includere le seguenti proprietà:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Aggiorna il file `clientlibs/clientlib-base/.content.xml` in **incorpora** la categoria `wknd.helloworld` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Distribuisci le modifiche in un’istanza locale di AEM utilizzando il plug-in per sviluppatori o le tue competenze Maven.

   >[!NOTE]
   >
   > I CSS e JavaScript vengono spesso memorizzati nella cache dal browser per motivi di prestazioni. Se non vedi immediatamente la modifica per la libreria client, esegui un aggiornamento rigido e cancella la cache del browser. Può essere utile utilizzare una finestra in incognito per garantire una nuova cache.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena appreso le nozioni di base sullo sviluppo dei componenti in Adobe Experience Manager!

### Passaggi successivi {#next-steps}

Acquisisci familiarità con le pagine e i modelli di Adobe Experience Manager nel capitolo successivo [Pagine e modelli](pages-templates.md). Scopri in che modo i componenti core vengono associati al progetto e apprendi configurazioni di policy avanzate di modelli modificabili per creare un modello Pagina articoli ben strutturato.

Visualizza il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente sul ramo Git `tutorial/component-basics-solution`.

