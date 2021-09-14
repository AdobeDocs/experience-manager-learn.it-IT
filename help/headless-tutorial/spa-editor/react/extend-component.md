---
title: Estendere un componente core | Guida introduttiva all'editor di SPA AEM e React
description: Scopri come estendere il modello JSON per un componente core esistente da utilizzare con l’editor di SPA AEM. Scopri come aggiungere proprietà e contenuto a un componente esistente è una tecnica potente per espandere le funzionalità di un’implementazione di AEM Editor SPA. Scopri come utilizzare il pattern di delega per l’estensione dei modelli Sling e delle funzioni di Sling Resource Merger.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1091'
ht-degree: 1%

---

# Estendere un componente core {#extend-component}

Scopri come estendere un componente core esistente da utilizzare con l’editor di SPA AEM. Scopri come estendere un componente esistente è una tecnica potente per personalizzare ed espandere le funzionalità di un’implementazione di AEM Editor .

## Obiettivo

1. Estendi un componente core esistente con proprietà e contenuto aggiuntivi.
2. Comprendere le nozioni di base dell’ereditarietà dei componenti con l’utilizzo di `sling:resourceSuperType`.
3. Scopri come sfruttare il [Pattern di delega](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) per i modelli Sling per riutilizzare la logica e le funzionalità esistenti.

## Cosa verrà creato

Questo capitolo illustra il codice aggiuntivo necessario per aggiungere una proprietà aggiuntiva a un componente `Image` standard per soddisfare i requisiti di un nuovo componente `Banner`. Il componente `Banner` contiene tutte le stesse proprietà del componente standard `Image`, ma include una proprietà aggiuntiva per consentire agli utenti di compilare il **Testo banner**.

![Componente banner per l’authoring finale](assets/extend-component/final-author-banner-component.png)

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment). A questo punto, si presume che gli utenti dell’esercitazione abbiano una conoscenza approfondita della funzione di editor di SPA AEM.

## Ereditarietà con Sling Resource Super Type {#sling-resource-super-type}

Per estendere un componente esistente, imposta una proprietà denominata `sling:resourceSuperType` nella definizione del componente.  `sling:resourceSuperType`è una  [](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) proprietà che può essere impostata sulla definizione di un componente AEM che punta a un altro componente. Questo imposta esplicitamente il componente per ereditare tutte le funzionalità del componente identificato come `sling:resourceSuperType`.

Per estendere il componente `Image` in `wknd-spa-react/components/image`, è necessario aggiornare il codice nel modulo `ui.apps` .

1. Crea una nuova cartella sotto il modulo `ui.apps` per `banner` in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. Sotto `banner` crea una definizione di componente (`.content.xml`) come segue:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Questo imposta `wknd-spa-react/components/banner` per ereditare tutte le funzionalità di `wknd-spa-react/components/image`.

## cq:editConfig {#cq-edit-config}

Il file `_cq_editConfig.xml` determina il comportamento di trascinamento nell’interfaccia utente di authoring AEM. Quando estendi il componente Immagine, è importante che il tipo di risorsa corrisponda al componente stesso.

1. Nel modulo `ui.apps` crea un altro file sotto `banner` denominato `_cq_editConfig.xml`.
1. Compilare `_cq_editConfig.xml` con il seguente XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. L’aspetto univoco del file è il nodo `<parameters>` che imposta resourceType su `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La maggior parte dei componenti non richiede un `_cq_editConfig`. I componenti immagine e i discendenti sono l&#39;eccezione.

## Estendi la finestra di dialogo {#extend-dialog}

Il nostro componente `Banner` richiede un campo di testo aggiuntivo nella finestra di dialogo per acquisire il `bannerText`. Poiché usiamo l’ereditarietà Sling, possiamo utilizzare le funzioni di [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) per sovrascrivere o estendere parti della finestra di dialogo. In questo esempio è stata aggiunta una nuova scheda alla finestra di dialogo per acquisire dati aggiuntivi da un autore per compilare il componente scheda.

1. Nel modulo `ui.apps`, sotto la cartella `banner`, crea una cartella denominata `_cq_dialog`.
1. Sotto `_cq_dialog` crea un file di definizione della finestra di dialogo `.content.xml`. Popolare con i seguenti elementi:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   La definizione XML di cui sopra creerà una nuova scheda denominata **Testo** e la ordinerà *prima* della scheda **Risorsa** esistente. Conterrà un singolo campo **Testo banner**.

1. La finestra di dialogo avrà un aspetto simile al seguente:

   ![Finestra di dialogo finale del banner](assets/extend-component/banner-dialog.png)

   Non è stato necessario definire le schede per **Risorsa** o **Metadati**. Vengono ereditate tramite la proprietà `sling:resourceSuperType` .

   Prima di poter visualizzare in anteprima la finestra di dialogo, è necessario implementare il componente SPA e la funzione `MapTo` .

## Implementare SPA componente {#implement-spa-component}

Per utilizzare il componente Banner con l’Editor di SPA, è necessario creare un nuovo componente SPA che verrà mappato su `wknd-spa-react/components/banner`. Questo verrà fatto nel modulo `ui.frontend` .

1. Nel modulo `ui.frontend` crea una nuova cartella per `Banner` in `ui.frontend/src/components/Banner`.
1. Crea un nuovo file denominato `Banner.js` sotto la cartella `Banner` . Popolare con i seguenti elementi:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
   
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if(BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   Questo componente SPA è associato al componente AEM `wknd-spa-react/components/banner` creato in precedenza.

1. Aggiorna `import-components.js` in `ui.frontend/src/components/import-components.js` per includere il nuovo componente `Banner` SPA:

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. A questo punto il progetto può essere distribuito in AEM e la finestra di dialogo può essere testata. Distribuisci il progetto utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Aggiorna i criteri del modello di SPA per aggiungere il componente `Banner` come **componente consentito**.

1. Passa a una pagina SPA e aggiungi il componente `Banner` a una delle pagine SPA:

   ![Aggiungi componente banner](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > La finestra di dialogo consente di salvare un valore per **Testo banner**, ma questo valore non viene riportato nel componente SPA. Per abilitare , è necessario estendere il modello Sling per il componente.

## Aggiungi interfaccia Java {#java-interface}

Per esporre i valori dalla finestra di dialogo del componente al componente React, è necessario aggiornare il modello Sling che compila il JSON per il componente `Banner`. Questo verrà fatto nel modulo `core` che contiene tutto il codice Java per il nostro progetto SPA.

Innanzitutto, creeremo una nuova interfaccia Java per `Banner` che estende l’ `Image` interfaccia Java .

1. Nel modulo `core` crea un nuovo file denominato `BannerModel.java` in `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Popolare `BannerModel.java` con quanto segue:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   Questo erediterà tutti i metodi dall&#39;interfaccia del componente core `Image` e aggiungerà un nuovo metodo `getBannerText()`.

## Implementare il modello Sling {#sling-model}

Quindi, implementa il modello Sling per l&#39;interfaccia `BannerModel` .

1. Nel modulo `core` crea un nuovo file denominato `BannerModelImpl.java` in `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. Popolare `BannerModelImpl.java` con quanto segue:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
       )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   
   }
   ```

   Osserva l’uso delle annotazioni `@Model` e `@Exporter` per garantire che il modello Sling possa essere serializzato come JSON tramite l’esportatore di modelli Sling.

   `BannerModelImpl.java` utilizza il pattern di  [delega per ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) i modelli Sling per evitare di riscrivere tutta la logica dal componente di base Immagine .

1. Osserva le seguenti righe:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   L’annotazione precedente crea un’istanza di un oggetto Immagine denominato `image` in base all’ `sling:resourceSuperType` ereditarietà del componente `Banner` .

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   È quindi possibile utilizzare semplicemente l&#39;oggetto `image` per implementare i metodi definiti dall&#39;interfaccia `Image` senza dover scrivere personalmente la logica. Questa tecnica viene utilizzata per `getSrc()`, `getAlt()` e `getTitle()`.

1. Apri una finestra terminale e distribuisci solo gli aggiornamenti al modulo `core` utilizzando il profilo Maven `autoInstallBundle` dalla directory `core`.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## Mettere tutto insieme {#put-together}

1. Torna a AEM e apri la pagina SPA con il componente `Banner` .
1. Aggiorna il componente `Banner` per includere **Testo banner**:

   ![Testo banner](assets/extend-component/banner-text-dialog.png)

1. Popolare il componente con un’immagine:

   ![Aggiungi immagine al banner, finestra di dialogo](assets/extend-component/banner-dialog-image.png)

   Salva gli aggiornamenti della finestra di dialogo.

1. Ora dovresti vedere il valore di cui è stato effettuato il rendering di **Testo banner**:

   ![Testo del banner visualizzato](assets/extend-component/banner-text-displayed.png)

1. Visualizza la risposta del modello JSON in: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e cerca il `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   Nota che il modello JSON viene aggiornato con coppie chiave/valore aggiuntive dopo l’implementazione del modello Sling in `BannerModelImpl.java`.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a estendere un componente AEM utilizzando e come modelli e finestre di dialogo Sling funzionano con il modello JSON.
