---
title: Creare un componente personalizzato | Guida introduttiva dell’Editor SPA di AEM e di Angular
description: Scopri come creare un componente personalizzato da utilizzare con l’Editor SPA di AEM. Scopri come sviluppare finestre di dialogo di authoring e modelli Sling per estendere il modello JSON e popolare un componente personalizzato.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
duration: 308
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# Creare un componente personalizzato {#custom-component}

Scopri come creare un componente personalizzato da utilizzare con l’Editor SPA di AEM. Scopri come sviluppare finestre di dialogo di authoring e modelli Sling per estendere il modello JSON e popolare un componente personalizzato.

## Obiettivo

1. Comprendi il ruolo dei modelli Sling nella manipolazione dell’API del modello JSON fornita da AEM.
2. Scopri come creare le finestre di dialogo dei componenti di AEM.
3. Scopri come creare un componente AEM **personalizzato** compatibile con il framework dell’editor SPA.

## Cosa verrà creato

L&#39;obiettivo dei capitoli precedenti era lo sviluppo di componenti SPA e il loro mapping a *esistenti* componenti core AEM. Questo capitolo illustra come creare ed estendere *nuovi* componenti AEM e manipolare il modello JSON gestito da AEM.

Un semplice `Custom Component` illustra i passaggi necessari per creare un nuovo componente AEM.

![Messaggio visualizzato in maiuscolo](assets/custom-component/message-displayed.png)

## Prerequisiti

Esaminare gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Implementa la base di codice in un’istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility), aggiungi il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installa il pacchetto finito per il [sito di riferimento WKND tradizionale](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite dal [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) sono riutilizzate nell&#39;applicazione a pagina singola WKND. È possibile installare il pacchetto utilizzando [Gestione pacchetti di AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Gestione pacchetti installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o estrarre il codice localmente passando al ramo `Angular/custom-component-solution`.

## Definire il componente AEM

Un componente AEM è definito come nodo e proprietà. Nel progetto, questi nodi e proprietà sono rappresentati come file XML nel modulo `ui.apps`. Creare quindi il componente AEM nel modulo `ui.apps`.

>[!NOTE]
>
> Un rapido aggiornamento sulle [nozioni di base dei componenti di AEM potrebbe essere utile](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=it).

1. Aprire la cartella `ui.apps` nell&#39;IDE desiderato.
2. Passare a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` e creare una cartella denominata `custom-component`.
3. Creare un file denominato `.content.xml` sotto la cartella `custom-component`. Popolare `custom-component/.content.xml` con quanto segue:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Crea definizione componente personalizzato](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifica che questo nodo è un componente di AEM.

   `jcr:title` è il valore visualizzato agli autori di contenuti e `componentGroup` determina il raggruppamento di componenti nell&#39;interfaccia utente di creazione.

4. Nella cartella `custom-component` creare un&#39;altra cartella denominata `_cq_dialog`.
5. Sotto la cartella `_cq_dialog` creare un file denominato `.content.xml` e popolarlo con il seguente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   ![Definizione componente personalizzato](assets/custom-component/dialog-custom-component-defintion.png)

   Il file XML sopra riportato genera una semplice finestra di dialogo per `Custom Component`. La parte critica del file è il nodo `<message>` interno. Questa finestra di dialogo contiene un `textfield` semplice denominato `Message` e mantiene il valore del campo di testo in una proprietà denominata `message`.

   Viene creato un modello Sling accanto a per esporre il valore della proprietà `message` tramite il modello JSON.

   >[!NOTE]
   >
   > Puoi visualizzare molti più [esempi di finestre di dialogo visualizzando le definizioni dei Componenti core](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Puoi anche visualizzare altri campi modulo, come `select`, `textarea`, `pathfield`, disponibili sotto `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Con un componente AEM tradizionale, in genere è necessario uno script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=it). Poiché l’applicazione a pagina singola esegue il rendering del componente, non è necessario alcuno script HTL.

## Creare il modello Sling

I modelli Sling sono Java™ &quot;POJO&quot; (Plain Old Java™ Objects) basati su annotazioni che facilitano la mappatura dei dati dalle variabili JCR a Java™. [I modelli Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=it#sling-models) in genere funzionano per incapsulare una logica di business lato server complessa per i componenti AEM.

Nel contesto dell&#39;Editor SPA, i modelli Sling espongono il contenuto di un componente tramite il modello JSON tramite una funzione che utilizza [Sling Model Exporter](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=it).

1. Nell&#39;IDE scelto aprire il modulo `core`. `CustomComponent.java` e `CustomComponentImpl.java` sono già stati creati e sottoposti a stub come parte del codice iniziale del capitolo.

   >[!NOTE]
   >
   > Se si utilizza l&#39;IDE di codice di Visual Studio, potrebbe essere utile installare [estensioni per Java™](https://code.visualstudio.com/docs/java/extensions).

2. Aprire l&#39;interfaccia Java™ `CustomComponent.java` in `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![Interfaccia CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Si tratta dell’interfaccia Java™ implementata dal modello Sling.

3. Aggiorna `CustomComponent.java` in modo che estenda l&#39;interfaccia `ComponentExporter`:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   L&#39;implementazione dell&#39;interfaccia `ComponentExporter` è un requisito affinché il modello Sling possa essere selezionato automaticamente dall&#39;API del modello JSON.

   L&#39;interfaccia `CustomComponent` include un singolo metodo getter `getMessage()`. Questo è il metodo che espone il valore della finestra di dialogo dell’autore tramite il modello JSON. Nel modello JSON vengono esportati solo i metodi getter con parametri vuoti `()`.

4. Apri `CustomComponentImpl.java` alle `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Implementazione dell&#39;interfaccia `CustomComponent`. L&#39;annotazione `@Model` identifica la classe Java™ come modello Sling. L&#39;annotazione `@Exporter` consente la serializzazione e l&#39;esportazione della classe Java™ tramite Sling Model Exporter.

5. Aggiornare la variabile statica `RESOURCE_TYPE` in modo che punti al componente AEM `wknd-spa-angular/components/custom-component` creato nell&#39;esercizio precedente.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Il tipo di risorsa del componente è ciò che associa il modello Sling al componente AEM e alla fine lo mappa al componente Angular.

6. Aggiungere il metodo `getExportedType()` alla classe `CustomComponentImpl` per restituire il tipo di risorsa del componente:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Questo metodo è necessario quando si implementa l&#39;interfaccia `ComponentExporter` ed espone il tipo di risorsa che consente il mapping al componente Angular.

7. Aggiornare il metodo `getMessage()` per restituire il valore della proprietà `message` persistente nella finestra di dialogo di authoring. Utilizzare l&#39;annotazione `@ValueMap` per mappare il valore JCR `message` a una variabile Java™:

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   Viene aggiunta un’ulteriore &quot;logica di business&quot; per restituire il valore del messaggio in maiuscolo. Questo consente di vedere la differenza tra il valore non elaborato memorizzato dalla finestra di dialogo di authoring e il valore esposto dal modello Sling.

   >[!NOTE]
   >
   > Puoi visualizzare [CustomComponentImpl.java completato qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Aggiornare il componente Angular

Il codice Angular per il componente personalizzato è già stato creato. Quindi, effettua alcuni aggiornamenti per mappare il componente Angular al componente AEM.

1. Nel modulo `ui.frontend` apri il file `ui.frontend/src/app/components/custom/custom.component.ts`
2. Osservare la riga `@Input() message: string;`. È previsto che il valore in maiuscolo trasformato sia mappato a questa variabile.
3. Importare l&#39;oggetto `MapTo` dal SDK JS dell&#39;editor di applicazioni a pagina singola di AEM e utilizzarlo per eseguire il mapping al componente AEM:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Aprire `cutom.component.html` e osservare che il valore di `{{message}}` è visualizzato sul lato di un tag `<h2>`.
5. Apri `custom.component.css` e aggiungi la seguente regola:

   ```css
   :host-context {
       display: block;
   }
   ```

   Affinché il segnaposto dell&#39;editor di AEM venga visualizzato correttamente quando il componente è vuoto, è necessario impostare `:host-context` o un altro `<div>` su `display: block;`.

6. Distribuisci gli aggiornamenti a un ambiente AEM locale dalla directory principale del progetto, utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Aggiornare il criterio del modello

Quindi, passa ad AEM per verificare gli aggiornamenti e consentire l&#39;aggiunta di `Custom Component` all&#39;applicazione a pagina singola.

1. Verifica la registrazione del nuovo modello Sling passando a [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Le due righe precedenti indicano che `CustomComponentImpl` è associato al componente `wknd-spa-angular/components/custom-component` e che è registrato tramite Sling Model Exporter.

2. Passa al modello per pagina SPA all&#39;indirizzo [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aggiornare il criterio Contenitore di layout per aggiungere il nuovo `Custom Component` come componente consentito:

   ![Aggiorna criterio contenitore layout](assets/custom-component/custom-component-allowed.png)

   Salvare le modifiche al criterio e osservare `Custom Component` come componente consentito:

   ![Componente personalizzato come componente consentito](assets/custom-component/custom-component-allowed-layout-container.png)

## Creare il componente personalizzato

Quindi, creare `Custom Component` utilizzando l&#39;editor SPA di AEM.

1. Passa a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In modalità `Edit`, aggiungere `Custom Component` a `Layout Container`:

   ![Inserisci nuovo componente](assets/custom-component/insert-custom-component.png)

3. Apri la finestra di dialogo del componente e immetti un messaggio contenente alcune lettere minuscole.

   ![Configura il componente personalizzato](assets/custom-component/enter-dialog-message.png)

   Questa è la finestra di dialogo creata in base al file XML precedente nel capitolo.

4. Salva le modifiche. Il messaggio visualizzato deve essere scritto in maiuscolo.

   ![Messaggio visualizzato in maiuscolo](assets/custom-component/message-displayed.png)

5. Visualizza il modello JSON passando a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Cerca `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Il valore JSON è impostato su tutte le lettere maiuscole in base alla logica aggiunta al modello Sling.

## Congratulazioni. {#congratulations}

Congratulazioni, hai imparato a creare un componente AEM personalizzato e come funzionano i modelli e le finestre di dialogo Sling con il modello JSON.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o estrarre il codice localmente passando al ramo `Angular/custom-component-solution`.

### Passaggi successivi {#next-steps}

[Estendere un componente core](extend-component.md) - Scopri come estendere un componente core esistente da utilizzare con l&#39;editor SPA di AEM. Scopri come aggiungere proprietà e contenuto a un componente esistente per espandere le funzionalità di un’implementazione dell’Editor SPA di AEM.
