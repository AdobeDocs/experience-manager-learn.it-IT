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

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

Aprite un nuovo terminale della riga di comando ed eseguite le seguenti operazioni.

1. In una directory vuota, clonate il repository [aem-guide-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > Facoltativamente, è possibile scaricare direttamente il ramo [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip).

1. Andate nella directory `aem-guides-wknd`:

   ```shell
   $ cd aem-guides-wknd
   ```

1. Passate al ramo `component-basics/start`:

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

1. Importa il progetto nell&#39;IDE desiderato seguendo le istruzioni per impostare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

## Authoring dei componenti {#component-authoring}

I componenti possono essere considerati come piccoli elementi di base modulari di una pagina Web. Per riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita mediante la finestra di dialogo di authoring. In seguito verrà creato un componente semplice e verrà analizzato il modo in cui i valori contenuti nella finestra di dialogo vengono AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Create una nuova pagina denominata **Component Basics** sotto **WKND Site** `>` **US** `>` **en**.
1. Aggiungete il **componente Hello World** alla pagina appena creata.
1. Aprire la finestra di dialogo del componente e immettere del testo. Salvare le modifiche per visualizzare il messaggio sulla pagina.
1. Passate alla modalità sviluppatore e visualizzate il Percorso contenuto in CRXDE-Lite, quindi controllate le proprietà dell’istanza del componente.
1. Utilizzare CRXDE-Lite per visualizzare lo script `cq:dialog` e `helloworld.html` situato in `/apps/wknd/components/content/helloworld`.

## HTML Template Language (HTL) {#htl-templates}

HTML Template Language o [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) è un linguaggio di modellazione lato server leggero utilizzato dai componenti AEM per il rendering del contenuto.

Successivamente verrà aggiornato lo script `HelloWorld` HTL per visualizzare un messaggio di saluto aggiuntivo prima del messaggio di testo.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Passate all&#39;IDE Eclipse e aprite il progetto nel modulo `ui.apps`.
1. Aprire il file `.content.xml` che definisce la finestra di dialogo per il componente `HelloWorld` in:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. Aggiornate la finestra di dialogo per aggiungere un campo di testo aggiuntivo denominato **Saluto** con un nome di `./greeting`:

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

1. Aprite il file `helloworld.html`, che rappresenta lo script HTL principale responsabile del rendering del componente `HelloWorld`, ubicato in:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. Aggiornate `helloworld.html` per eseguire il rendering del valore del campo di testo **Greeting** come parte di un tag `H1`:

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

Verranno quindi eseguiti alcuni aggiornamenti al modello `HelloWorldModel` Sling per applicare una qualche logica di business ai valori memorizzati nel JCR prima di inviarli alla pagina.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. Aprire il file `HelloWorldModel.java`, ovvero il modello Sling utilizzato con il componente `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Aggiungere le righe seguenti alla classe `HelloWorldModel` per mappare i valori delle proprietà JCR del componente `greeting` e `text` alle variabili Java:

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

1. Aggiungete il seguente metodo `getGreeting()` alla classe `HelloWorldModel`, che restituisce il valore della proprietà denominata `greeting`. Questo metodo aggiunge la logica aggiuntiva per restituire un valore String &quot;Hello&quot; se la proprietà `greeting` è null o vuota:

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. Aggiungete il seguente metodo `getTextUpperCase()` alla classe `HelloWorldModel`, che restituisce il valore della proprietà denominata `text`. Questo metodo trasforma la stringa in tutti i caratteri maiuscoli.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Aggiornare il file `helloworld.html` in `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` per utilizzare i metodi appena creati del modello `HelloWorld`:

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

A questo punto, includeremo alcuni stili CSS per il componente `HelloWorld` per comprendere le nozioni di base delle librerie lato client.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. in `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` creare un nuovo nodo denominato `clientlibs` con un tipo di nodo `cq:ClientLibraryFolder`.
1. Crea una struttura di cartelle e file come segue sotto `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. Compilare `helloworld/clientlibs/css/helloworld.css` con i seguenti elementi:

   ```css
   .cmp-helloworld h1 {
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
   console.log("hello world!");
   ```

1. Compilare `helloworld/clientlibs/js.txt` con i seguenti elementi:

   ```plain
   #base=js
   helloworld.js
   ```

1. Aggiornare le proprietà del nodo `clientlibs` in modo da includere le due seguenti proprietà:

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

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in corrispondenza del blocco Git `component-basics/solution`.

1. Duplicare il repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `component-basics/solution`
