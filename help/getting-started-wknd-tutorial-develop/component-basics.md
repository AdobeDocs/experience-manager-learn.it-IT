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
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 2%

---


# Nozioni di base sui componenti {#component-basics}

Questo capitolo illustra la tecnologia alla base di un componente di Adobe Experience Manager (AEM) Sites attraverso un semplice `HelloWorld` esempio. Verranno apportate piccole modifiche a un componente esistente, con argomenti quali authoring, HTL, Sling Models e librerie lato client.

## Prerequisiti {#prerequisites}

Esaminate le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

## Obiettivo {#objective}

1. Scoprite il ruolo dei modelli HTL e Sling Models per eseguire il rendering HTML in modo dinamico.
1. Comprendere in che modo le finestre di dialogo vengono utilizzate per facilitare la creazione di contenuto.
1. Scoprite le nozioni di base delle librerie lato client per includere CSS e JavaScript per il supporto di un componente.

## Cosa verrà creato {#what-you-will-build}

In questo capitolo verranno apportate diverse modifiche a un `HelloWorld` componente molto semplice. Durante il processo di aggiornamento del `HelloWorld` componente, verranno fornite informazioni sulle aree chiave dello sviluppo AEM componente.

## Progetto iniziale capitolo {#starter-project}

Questo capitolo si basa su un progetto generico generato da Project Archetype [](https://github.com/adobe/aem-project-archetype)AEM. Guardate il video seguente e controllate i [prerequisiti](#prerequisites) per iniziare!

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

Aprite un nuovo terminale della riga di comando ed eseguite le seguenti operazioni.

1. In una directory vuota, clonate l&#39;archivio [aem-guide-wknd](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > Facoltativamente, potete scaricare direttamente il [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) ramo.

1. Andate nella `aem-guides-wknd` directory:

   ```shell
   $ cd aem-guides-wknd
   ```

1. Passate al `component-basics/start` ramo:

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. Create e distribuite il progetto in un&#39;istanza locale di AEM con il seguente comando:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. Importa il progetto nell&#39;IDE desiderato seguendo le istruzioni per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

## Authoring dei componenti {#component-authoring}

I componenti possono essere considerati come piccoli elementi di base modulari di una pagina Web. Per riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita mediante la finestra di dialogo di authoring. In seguito verrà creato un componente semplice e verrà analizzato il modo in cui i valori contenuti nella finestra di dialogo vengono AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Create una nuova pagina denominata **Component Basics** (Nozioni di base sui **componenti) sotto il sito** `>` WKND **US** `>`**it**.
1. Aggiungete il componente **Hello World** alla pagina appena creata.
1. Aprire la finestra di dialogo del componente e immettere del testo. Salvare le modifiche per visualizzare il messaggio sulla pagina.
1. Passate alla modalità sviluppatore e visualizzate il Percorso contenuto in CRXDE-Lite, quindi controllate le proprietà dell’istanza del componente.
1. Utilizzare CRXDE-Lite per visualizzare lo `cq:dialog` script e `helloworld.html` situato in `/apps/wknd/components/content/helloworld`.

## HTML Template Language (HTL) {#htl-templates}

HTML Template Language o [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) è un linguaggio di modellazione lato server leggero utilizzato dai componenti AEM per il rendering del contenuto.

Successivamente verrà aggiornato lo script `HelloWorld` HTL per visualizzare un messaggio di saluto aggiuntivo prima del messaggio di testo.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Passate all&#39;IDE Eclipse e aprite il progetto al `ui.apps` modulo.
1. Aprite il `.content.xml` file che definisce la finestra di dialogo per il `HelloWorld` componente in:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. Aggiornate la finestra di dialogo per aggiungere un altro campo di testo denominato **Saluto** con un nome di `./greeting`:

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
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
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

1. Aprite il file `helloworld.html`, che rappresenta lo script HTL principale responsabile per il rendering del `HelloWorld` componente, ubicato in:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. Aggiorna `helloworld.html` per eseguire il rendering del valore del campo di testo **Saluto** come parte di un `H1` tag:

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Distribuite le modifiche in un&#39;istanza locale di AEM utilizzando il plug-in per sviluppatori Eclipse o utilizzando le vostre abilità Maven.

## Modelli Sling {#sling-models}

I modelli Sling sono modelli Java &quot;POJO&quot; basati su annotazioni (Plain Old Java Objects) che semplificano la mappatura dei dati dalle variabili JCR a Java e forniscono una serie di altre caratteristiche quando si sviluppano nel contesto di AEM.

Verranno quindi introdotti alcuni aggiornamenti al modello `HelloWorldModel` Sling per applicare una qualche logica di business ai valori memorizzati nel JCR prima di inviarli alla pagina.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. Aprire il file `HelloWorldModel.java`, ovvero il modello Sling utilizzato con il `HelloWorld` componente.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Aggiungete le seguenti righe alla `HelloWorldModel` classe per mappare i valori delle proprietà JCR del componente `greeting` e `text` alle variabili Java:

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. Aggiungete il seguente metodo `getGreeting()` alla `HelloWorldModel` classe, che restituisce il valore della proprietà denominata `greeting`. Questo metodo aggiunge la logica aggiuntiva per restituire un valore String &quot;Hello&quot; se la proprietà `greeting` è null o blank:

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. Aggiungete il seguente metodo `getTextUpperCase()` alla `HelloWorldModel` classe, che restituisce il valore della proprietà denominata `text`. Questo metodo trasforma la stringa in tutti i caratteri maiuscoli.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Aggiornare il file `helloworld.html` in `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` modo da utilizzare i metodi appena creati del `HelloWorld` modello:

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Distribuite le modifiche in un&#39;istanza locale di AEM utilizzando il plug-in per sviluppatori Eclipse o utilizzando le vostre abilità Maven.

## Librerie lato client {#client-side-libraries}

Client-Side Libraries, clientlibs in breve, fornisce un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un&#39;implementazione  AEM Sites. Le librerie lato client sono il metodo standard per includere CSS e JavaScript in una pagina di AEM.

Verranno quindi inclusi alcuni stili CSS per il `HelloWorld` componente, al fine di comprendere le nozioni di base delle librerie lato client.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. sotto `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` creare un nuovo nodo denominato `clientlibs` con un tipo di nodo `cq:ClientLibraryFolder`.
1. Creare una struttura di cartelle e file come segue `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. Compilate `helloworld/clientlibs/css/helloworld.css` con le seguenti opzioni:

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. Compilate `helloworld/clientlibs/css.txt` con le seguenti opzioni:

   ```plain
   #base=css
   helloworld.css
   ```

1. Compilate `helloworld/clientlibs/js/helloworld.js` con le seguenti opzioni:

   ```js
   console.log("hello world!");
   ```

1. Compilate `helloworld/clientlibs/js.txt` con le seguenti opzioni:

   ```plain
   #base=js
   helloworld.js
   ```

1. Aggiorna le proprietà del `clientlibs` nodo per includere le due proprietà seguenti:

   | Nome | Tipo | Valore |
   |------|------|-------|
   | categorie | Stringa | wknd.base |
   | allowProxy | Booleano | vero |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Distribuite le modifiche in un&#39;istanza locale di AEM utilizzando il plug-in per sviluppatori Eclipse o utilizzando le vostre abilità Maven.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena imparato le basi dello sviluppo di componenti in Adobe Experience Manager!

### Passaggi successivi {#next-steps}

Acquisisci familiarità con le pagine e i modelli Adobe Experience Manager nel capitolo successivo [Pagine e modelli](pages-templates.md). Scopri in che modo i componenti core sono associati al progetto e impara le configurazioni di criteri avanzate dei modelli modificabili per creare un modello di pagina articolo ben strutturato.

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in Git brach `component-basics/solution`.

1. Duplicate l&#39;archivio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) .
1. Estrarre il `component-basics/solution` ramo
