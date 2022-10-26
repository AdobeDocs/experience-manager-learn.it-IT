---
title: Creare un componente personalizzato | Guida introduttiva all’editor di SPA AEM e all’Angular
description: Scopri come creare un componente personalizzato da utilizzare con l’editor SPA AEM. Scopri come sviluppare le finestre di dialogo degli autori e i modelli Sling per estendere il modello JSON a un componente personalizzato.
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 3%

---

# Creare un componente personalizzato {#custom-component}

Scopri come creare un componente personalizzato da utilizzare con l’editor SPA AEM. Scopri come sviluppare le finestre di dialogo degli autori e i modelli Sling per estendere il modello JSON a un componente personalizzato.

## Obiettivo

1. Comprendi il ruolo dei modelli Sling nella manipolazione dell’API del modello JSON fornita da AEM.
2. Scopri come creare finestre di dialogo dei componenti AEM.
3. Scopri come creare un **personalizzato** Componente AEM compatibile con il framework dell’editor SPA.

## Cosa verrà creato

I capitoli precedenti erano incentrati sullo sviluppo di componenti SPA e la loro mappatura su *esistente* AEM componenti core. Questo capitolo si concentra su come creare ed estendere *nuovo* AEM i componenti e manipolate il modello JSON servito da AEM.

Semplice `Custom Component` illustra i passaggi necessari per creare un nuovo componente AEM.

![Messaggio visualizzato in Tutte le maiuscole](assets/custom-component/message-displayed.png)

## Prerequisiti

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi le `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installa il pacchetto finito per il tradizionale [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite da [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) vengono riutilizzati nel SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o controlla il codice localmente passando al ramo `Angular/custom-component-solution`.

## Definire il componente AEM

Un componente AEM è definito come un nodo e proprietà. Nel progetto, questi nodi e proprietà sono rappresentati come file XML nel `ui.apps` modulo . Quindi, crea il componente AEM nel `ui.apps` modulo .

>[!NOTE]
>
> Un aggiornamento rapido sul [nozioni di base sui componenti AEM possono essere utili](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Apri `ui.apps` nell’IDE che preferisci.
2. Passa a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` e crea una cartella denominata `custom-component`.
3. Crea un file denominato `.content.xml` sotto il `custom-component` cartella. Popolare `custom-component/.content.xml` con le seguenti caratteristiche:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Crea definizione di componente personalizzato](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifica che questo nodo è un componente AEM.

   `jcr:title` è il valore visualizzato agli autori di contenuti e al `componentGroup` determina il raggruppamento di componenti nell’interfaccia utente di authoring.

4. Sotto `custom-component` cartella, creare un&#39;altra cartella denominata `_cq_dialog`.
5. Sotto `_cq_dialog` creare un file denominato `.content.xml` e popolalo con i seguenti elementi:

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

   ![Definizione del componente personalizzato](assets/custom-component/dialog-custom-component-defintion.png)

   Il file XML di cui sopra genera una semplice finestra di dialogo per `Custom Component`. La parte critica del file è l&#39;interno `<message>` nodo. Questa finestra di dialogo contiene un `textfield` denominato `Message` e persistere il valore del textifeld in una proprietà denominata `message`.

   Viene creato un modello Sling accanto a per esporre il valore del `message` tramite il modello JSON.

   >[!NOTE]
   >
   > Puoi visualizzare molto di più [esempi di finestre di dialogo visualizzando le definizioni dei componenti core](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). È inoltre possibile visualizzare campi modulo aggiuntivi, come `select`, `textarea`, `pathfield`, disponibile sotto `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Con un componente AEM tradizionale, un [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) in genere è richiesto uno script. Poiché il SPA esegue il rendering del componente, non è necessario alcuno script HTL.

## Creare il modello Sling

I modelli Sling sono &quot;POJO&quot; Java™ basati su annotazioni (Plain Old Java™ Objects) che facilitano la mappatura dei dati dalle variabili JCR alle variabili Java™. [Modelli Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) in genere funziona per incapsulare logiche aziendali complesse lato server per i componenti AEM.

Nel contesto dell’editor di SPA, i modelli Sling espongono il contenuto di un componente attraverso il modello JSON attraverso una funzione utilizzando [Esportatore di modelli Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=it).

1. Nell’IDE che preferisci, apri le `core` modulo . `CustomComponent.java` e `CustomComponentImpl.java` sono già stati creati e inseriti come parte del codice iniziale del capitolo.

   >[!NOTE]
   >
   > Se si utilizza l&#39;IDE di codice di Visual Studio, può essere utile installare [estensioni per Java™](https://code.visualstudio.com/docs/java/extensions).

2. Apri l&#39;interfaccia Java™ `CustomComponent.java` a `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![Interfaccia CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Questa è l&#39;interfaccia Java™ implementata da Sling Model.

3. Aggiorna `CustomComponent.java` in modo da estendere il `ComponentExporter` interfaccia:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   Implementazione `ComponentExporter` L’interfaccia è un requisito affinché il modello Sling venga automaticamente acquisito dall’API del modello JSON.

   La `CustomComponent` l&#39;interfaccia include un singolo metodo getter `getMessage()`. Questo è il metodo che espone il valore della finestra di dialogo di authoring attraverso il modello JSON. Solo metodi getter con parametri vuoti `()` vengono esportati nel modello JSON.

4. Apri `CustomComponentImpl.java` a `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Questa è l&#39;attuazione del `CustomComponent` interfaccia. La `@Model` annotazione identifica la classe Java™ come modello Sling. La `@Exporter` l’annotazione consente di serializzare ed esportare la classe Java™ tramite Sling Model Exporter.

5. Aggiorna la variabile statica `RESOURCE_TYPE` per puntare al componente AEM `wknd-spa-angular/components/custom-component` creato nell&#39;esercizio precedente.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Il tipo di risorsa del componente è ciò che lega il modello Sling al componente AEM e, in definitiva, viene mappato sul componente Angular.

6. Aggiungi il `getExportedType()` al `CustomComponentImpl` per restituire il tipo di risorsa componente:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Questo metodo è necessario per implementare `ComponentExporter` e espone il tipo di risorsa che consente la mappatura al componente Angular.

7. Aggiorna `getMessage()` per restituire il valore del `message` proprietà persistita dalla finestra di dialogo dell&#39;autore. Utilizza la `@ValueMap` annotazione è la mappa del valore JCR `message` a una variabile Java™:

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

   Viene aggiunta una nuova &quot;logica di business&quot; per restituire il valore del messaggio in maiuscolo. Questo ci permette di vedere la differenza tra il valore non elaborato memorizzato dalla finestra di dialogo dell’autore e il valore esposto dal modello Sling.

   >[!NOTE]
   È possibile visualizzare [Ho finito CustomComponentImpl.java qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Aggiornare il componente Angular

Il codice di Angular per il componente personalizzato è già stato creato. Quindi, effettua alcuni aggiornamenti per mappare il componente Angular al componente AEM.

1. In `ui.frontend` aprire il file `ui.frontend/src/app/components/custom/custom.component.ts`
2. Osserva la `@Input() message: string;` linea. È previsto che il valore in maiuscolo trasformato sia mappato a questa variabile.
3. Importa `MapTo` dall’SDK AEM di JS per l’editor di SPA e utilizzalo per eseguire il mapping al componente AEM:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Apri `cutom.component.html` e osservano che il valore di `{{message}}` viene visualizzato accanto a un `<h2>` tag .
5. Apri `custom.component.css` e aggiungi la seguente regola:

   ```css
   :host-context {
       display: block;
   }
   ```

   Affinché il segnaposto dell’editor di AEM venga visualizzato correttamente quando il componente è vuoto, il `:host-context` o altro `<div>` deve essere impostato su `display: block;`.

6. Distribuisci gli aggiornamenti in un ambiente AEM locale dalla directory principale della directory di progetto, utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Aggiornare i criteri dei modelli

Quindi, accedi a AEM per verificare gli aggiornamenti e consentire il `Custom Component` da aggiungere al SPA.

1. Verifica la registrazione del nuovo modello Sling passando a [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Dovresti vedere le due righe precedenti che indicano il `CustomComponentImpl` è associato al `wknd-spa-angular/components/custom-component` e che è registrato tramite l’esportatore di modelli Sling.

2. Passa al modello di pagina SPA in [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aggiornare i criteri del Contenitore di layout per aggiungere il nuovo `Custom Component` come componente consentito:

   ![Aggiorna i criteri dei contenitori di layout](assets/custom-component/custom-component-allowed.png)

   Salva le modifiche al criterio e osserva il `Custom Component` come componente consentito:

   ![Componente personalizzato come componente consentito](assets/custom-component/custom-component-allowed-layout-container.png)

## Creare il componente personalizzato

Quindi, crea le `Custom Component` utilizzando l’editor SPA AEM.

1. Passa a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` aggiungi la `Custom Component` al `Layout Container`:

   ![Inserisci nuovo componente](assets/custom-component/insert-custom-component.png)

3. Apri la finestra di dialogo del componente e inserisci un messaggio contenente lettere minuscole.

   ![Configurare il componente personalizzato](assets/custom-component/enter-dialog-message.png)

   Questa è la finestra di dialogo creata in base al file XML precedente nel capitolo.

4. Salva le modifiche. Il messaggio visualizzato è in maiuscolo.

   ![Messaggio visualizzato in Tutte le maiuscole](assets/custom-component/message-displayed.png)

5. Visualizzare il modello JSON passando a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Cerca `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Il valore JSON viene impostato su tutte le lettere maiuscole in base alla logica aggiunta al modello Sling.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a creare un componente AEM personalizzato e come funzionano i modelli e le finestre di dialogo Sling con il modello JSON.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o controlla il codice localmente passando al ramo `Angular/custom-component-solution`.

### Passaggi successivi {#next-steps}

[Estendere un componente core](extend-component.md) - Scopri come estendere un componente core esistente da utilizzare con l’editor di SPA AEM. Scopri come aggiungere proprietà e contenuto a un componente esistente è una tecnica potente per espandere le funzionalità di un’implementazione di AEM Editor SPA.
