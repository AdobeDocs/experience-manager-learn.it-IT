---
title: Aggiornare il progetto di AEM dello stack completo per utilizzare la pipeline front-end
description: Scopri come aggiornare il progetto AEM dello stack completo per abilitarlo per la pipeline front-end, in modo che possa generare e distribuire solo gli artefatti front-end.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---


# Aggiornare il progetto di AEM dello stack completo per utilizzare la pipeline front-end {#update-project-enable-frontend-pipeline}

In questo capitolo, apporteremo modifiche alla configurazione __Progetto WKND Sites__ per utilizzare la pipeline front-end per distribuire JavaScript e CSS, anziché richiedere un’esecuzione completa della pipeline full-stack. Questo consente di separare il ciclo di vita dello sviluppo e dell&#39;implementazione degli artefatti front-end e back-end, consentendo un processo di sviluppo iterativo più rapido nel suo complesso.

## Obiettivi {#objectives}

* Aggiornare il progetto full-stack per utilizzare la pipeline front-end

## Panoramica delle modifiche di configurazione nel progetto di AEM a stack completo

>[!VIDEO](https://video.tv.adobe.com/v/3409419/)

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che tu abbia rivisto il [Modulo &#39;ui.frontend&#39;](./review-uifrontend-module.md).


## Modifiche al progetto di AEM dello stack completo

Sono disponibili tre modifiche di configurazione relative al progetto e una modifica dello stile da distribuire per un’esecuzione di test, quindi in totale quattro modifiche specifiche nel progetto WKND per abilitarlo per il contratto della pipeline front-end.

1. Rimuovi `ui.frontend` modulo dal ciclo di compilazione a stack completo

   * In, la directory principale del progetto WKND Sites `pom.xml` commenta `<module>ui.frontend</module>` voce del sottomodulo.

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * E commenta la dipendenza correlata dal `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. Preparare `ui.frontend` modulo per il contratto della pipeline front-end aggiungendo due nuovi file di configurazione del webpack.

   * Copia l&#39;esistente `webpack.common.js` come `webpack.theme.common.js`, modifica `output` proprietà e `MiniCssExtractPlugin`, `CopyWebpackPlugin` parametri di configurazione plugin come segue:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'clientlib-[name]/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Copia l&#39;esistente `webpack.prod.js` come `webpack.theme.prod.js`, e modificare le `common` posizione della variabile nel file sopra indicato come

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >Le due modifiche di configurazione &quot;webpack&quot; di cui sopra devono avere nomi di file di output e cartelle diversi, in modo da poter distinguere facilmente tra gli artefatti front-end della pipeline clientlib (Full-stack) e quelli generati dal tema (front-end).
   >
   >Come hai indovinato, le modifiche di cui sopra possono essere saltate per utilizzare anche le configurazioni del webpack esistenti, ma sono necessarie le seguenti modifiche.
   >
   >Dipende da come volete nominarli o organizzarli.


   * In `package.json` file, assicurati che  `name` il valore della proprietà è lo stesso del nome del sito del `/conf` nodo. E sotto `scripts` proprietà, `build` script che indica come creare i file front-end da questo modulo.

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. Preparare `ui.content` modulo per la pipeline front-end aggiungendo due configurazioni Sling.

   * Crea un nuovo file in `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - include tutti i file front-end che il `ui.frontend` il modulo genera sotto `dist` utilizzando il processo di creazione del webpack.

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    Vedi il [Configurazione HtmlPageItems](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) in __AEM progetto WKND Sites__.


   * Secondo `com.adobe.aem.wcm.site.manager.config.SiteConfig` con `themePackageName` uguale a `package.json` e `name` valore della proprietà e `siteTemplatePath` indica un `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` valore del percorso stub.

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    Vedi il [Configurazione sito](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) in __AEM progetto WKND Sites__.

1. Un tema o gli stili vengono modificati per essere distribuiti tramite una pipeline front-end per un’esecuzione di test. `text-color` all&#39;Adobe rosso (o puoi scegliere il tuo) aggiornando il `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Infine, invia queste modifiche all’archivio Git Adobe del tuo programma.


>[!AVAILABILITY]
>
> Queste modifiche sono disponibili su GitHub all’interno di [__tubazione front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) della filiale __AEM progetto WKND Sites__.


## Congratulazioni! {#congratulations}

Congratulazioni, hai aggiornato il progetto WKND Sites per abilitarlo per il contratto di pipeline front-end.

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [Distribuzione tramite la pipeline front-end](create-frontend-pipeline.md), puoi creare ed eseguire una pipeline front-end e verificare come __allontanato__ dalla consegna di risorse front-end basate su &quot;/etc.clientlibs&quot;.
