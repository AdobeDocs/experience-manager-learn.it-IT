---
title: 'Guida introduttiva ad AEM Sites: nozioni di base sui componenti'
description: Comprendi la tecnologia sottostante di un componente Adobe Experience Manager (AEM) Sites attraverso un semplice esempio di "HelloWorld". Vengono esplorati gli argomenti di HTL, modelli Sling, librerie lato client e finestre di dialogo per l’authoring.
version: 6.5, Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
duration: 1715
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 0%

---

# Nozioni di base sui componenti {#component-basics}

In questo capitolo, analizziamo la tecnologia di base di un componente Sites di Adobe Experience Manager (AEM) tramite un semplice esempio di `HelloWorld`. Sono state apportate piccole modifiche a un componente esistente, riguardanti argomenti quali authoring, HTL, modelli Sling e librerie lato client.

## Prerequisiti {#prerequisites}

Esaminare gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](./overview.md#local-dev-environment).

L&#39;IDE utilizzato nei video è [Visual Studio Code](https://code.visualstudio.com/) e il plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync).

## Obiettivo {#objective}

1. Scopri il ruolo dei modelli HTL e dei modelli Sling per il rendering dinamico di HTML.
1. Scopri come le finestre di dialogo vengono utilizzate per facilitare l’authoring dei contenuti.
1. Scopri le nozioni di base delle librerie lato client per includere CSS e JavaScript per supportare un componente.

## Cosa intendi creare {#what-build}

In questo capitolo vengono apportate diverse modifiche a un semplice componente `HelloWorld`. Durante l&#39;aggiornamento del componente `HelloWorld`, si apprendono le aree chiave dello sviluppo dei componenti AEM.

## Progetto capitolo iniziale {#starter-project}

Questo capitolo si basa su un progetto generico generato da [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype). Guarda il video seguente e controlla i [prerequisiti](#prerequisites) per iniziare.

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per estrarre il progetto iniziale.

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

Apri un nuovo terminale della riga di comando ed esegui le azioni seguenti.

1. In una directory vuota, clona l’archivio [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > È possibile continuare a utilizzare il progetto generato nel capitolo precedente, [Configurazione del progetto](./project-setup.md).

1. Passare alla cartella `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Genera e implementa il progetto in un’istanza locale dell’AEM con il seguente comando:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importa il progetto nell&#39;IDE preferito seguendo le istruzioni per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

## Authoring dei componenti {#component-authoring}

I componenti possono essere considerati come piccoli blocchi predefiniti modulari di una pagina web. Per poter riutilizzare i componenti, questi devono essere configurabili. Questa operazione viene eseguita tramite la finestra di dialogo di authoring. Ora creiamo un componente semplice e analizziamo come i valori provenienti dalla finestra di dialogo vengono mantenuti nell’AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Crea una pagina denominata **Nozioni di base sui componenti** sotto **Sito WKND** `>` **US** `>` **en**.
1. Aggiungere il **componente Hello World** alla pagina appena creata.
1. Apri la finestra di dialogo del componente e immetti del testo. Salva le modifiche per visualizzare il messaggio visualizzato nella pagina.
1. Passa alla modalità sviluppatore e visualizza il Percorso contenuto in CRXDE-Lite, quindi controlla le proprietà dell’istanza del componente.
1. Utilizzare CRXDE-Lite per visualizzare lo script `cq:dialog` e `helloworld.html` da `/apps/wknd/components/content/helloworld`.

## HTL (HTML Template Language) e finestre di dialogo {#htl-dialogs}

HTML Template Language o **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** è un linguaggio di modelli lato server leggero utilizzato dai componenti AEM per il rendering del contenuto.

**Le finestre di dialogo** definiscono le configurazioni disponibili che possono essere create per un componente.

Aggiorniamo quindi lo script HTL `HelloWorld` per visualizzare un saluto aggiuntivo prima del messaggio di testo.

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Passare all&#39;IDE e aprire il progetto al modulo `ui.apps`.
1. Aprire il file `helloworld.html` e aggiornare il markup HTML.
1. Utilizza gli strumenti IDE come [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) per sincronizzare la modifica del file con l&#39;istanza AEM locale.
1. Torna al browser e osserva come è cambiato il rendering del componente.
1. Apri il file `.content.xml` che definisce la finestra di dialogo per il componente `HelloWorld` in:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Aggiorna la finestra di dialogo per aggiungere un campo di testo aggiuntivo denominato **Titolo** con nome `./title`:

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

1. Riapri il file `helloworld.html`, che rappresenta lo script HTL principale responsabile del rendering del componente `HelloWorld` dal percorso seguente:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Aggiorna `helloworld.html` per eseguire il rendering del valore del campo di testo **Saluto** come parte di un tag `H1`:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Distribuisci le modifiche in un’istanza locale dell’AEM utilizzando il plug-in per sviluppatori o Maven.

## Modelli Sling {#sling-models}

I modelli Sling sono Java™ &quot;POJO&quot; (Plain Old Java™ Objects) basati su annotazioni che facilitano la mappatura dei dati dalle variabili JCR a Java™. Forniscono anche molti altri vantaggi quando si sviluppano nel contesto dell&#39;AEM.

Quindi, apportiamo alcuni aggiornamenti al modello Sling `HelloWorldModel` per applicare una logica di business ai valori memorizzati nel JCR prima di eseguirne l&#39;output sulla pagina.

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. Aprire il file `HelloWorldModel.java`, che è il modello Sling utilizzato con il componente `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Aggiungi le seguenti istruzioni di importazione:

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Aggiornare l&#39;annotazione `@Model` per utilizzare un `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Aggiungi le righe seguenti alla classe `HelloWorldModel` per mappare i valori delle proprietà JCR `title` e `text` del componente alle variabili Java™:

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

1. Aggiungere il seguente metodo `getTitle()` alla classe `HelloWorldModel`, che restituisce il valore della proprietà denominata `title`. Questo metodo aggiunge la logica aggiuntiva per restituire il valore String &quot;Default Value here!&quot;. se la proprietà `title` è null o vuota:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Aggiungere il seguente metodo `getText()` alla classe `HelloWorldModel`, che restituisce il valore della proprietà denominata `text`. Questo metodo trasforma la stringa in tutti i caratteri maiuscoli.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Genera e distribuisci il bundle dal modulo `core`:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Per AEM 6.4/6.5 utilizzare `mvn clean install -PautoInstallBundle -Pclassic`

1. Aggiornare il file `helloworld.html` in `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` per utilizzare i metodi appena creati del modello `HelloWorld`.

   Viene creata un&#39;istanza del modello `HelloWorld` per questa istanza del componente tramite la direttiva HTL: `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"`. L&#39;istanza verrà salvata nella variabile `model`.

   L&#39;istanza del modello `HelloWorld` è ora disponibile in HTL tramite la variabile `model` utilizzando `HelloWord`. Le chiamate di questi metodi possono utilizzare una sintassi di metodo abbreviata, ad esempio: `${model.getTitle()}` può essere abbreviato in `${model.title}`.

   Analogamente, tutti gli script HTL vengono inseriti con [oggetti globali](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) a cui è possibile accedere utilizzando la stessa sintassi degli oggetti del modello Sling.

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
   </div>
   ```

1. Implementa le modifiche in un’istanza locale dell’AEM utilizzando il plug-in per sviluppatori Eclipse o utilizzando le tue competenze Maven.

## Librerie lato client {#client-side-libraries}

Le librerie lato client, `clientlibs` in breve, forniscono un meccanismo per organizzare e gestire i file CSS e JavaScript necessari per un&#39;implementazione AEM Sites. Le librerie lato client sono il modo standard per includere CSS e JavaScript in una pagina nell’AEM.

Il modulo [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) è un progetto [webpack](https://webpack.js.org/) disaccoppiato integrato nel processo di compilazione. Questo consente l’utilizzo delle librerie front-end più popolari come Sass, LESS e TypeScript. Il modulo `ui.frontend` viene approfondito nel [capitolo Librerie lato client](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

Aggiornare quindi gli stili CSS per il componente `HelloWorld`.

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Aprire una finestra del terminale e passare alla directory `ui.frontend`

1. Essendo nella directory `ui.frontend`, eseguire il comando `npm install npm-run-all --save-dev` per installare il modulo del nodo [npm-run-all](https://www.npmjs.com/package/npm-run-all). Questo passaggio è **obbligatorio per il progetto AEM** generato da Archetipo 39. Nella prossima versione di Archetipo, non è necessario.

1. Eseguire quindi il comando `npm run watch`:

   ```shell
   $ npm run watch
   ```

1. Passare all&#39;IDE e aprire il progetto al modulo `ui.frontend`.
1. Aprire il file `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. Aggiorna il file per visualizzare un titolo rosso:

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. Nel terminale dovrebbe essere presente un&#39;attività che indica che il modulo `ui.frontend` sta compilando e sincronizzando le modifiche con l&#39;istanza locale dell&#39;AEM.

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. Torna al browser e osserva che il colore del titolo è cambiato.

   ![Aggiornamento nozioni di base dei componenti](assets/component-basics/color-update.png)

## Congratulazioni. {#congratulations}

Congratulazioni, hai imparato le nozioni di base sullo sviluppo dei componenti in Adobe Experience Manager.

### Passaggi successivi {#next-steps}

Acquisisci familiarità con le pagine e i modelli di Adobe Experience Manager nel prossimo capitolo [Pagine e modelli](pages-templates.md). Scopri in che modo i Componenti core vengono inseriti come proxy nel progetto e scopri le configurazioni avanzate dei criteri dei modelli modificabili per creare un modello di pagina di articolo ben strutturato.

Visualizza il codice finito in [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente in nel ramo Git `tutorial/component-basics-solution`.
