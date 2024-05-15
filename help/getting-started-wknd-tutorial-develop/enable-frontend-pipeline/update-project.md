---
title: Aggiorna il progetto AEM full stack per utilizzare la pipeline front-end
description: Scopri come aggiornare il progetto AEM full stack per abilitarlo per la pipeline front-end, in modo da creare e distribuire solo gli artefatti front-end.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 307
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# Aggiorna il progetto AEM full stack per utilizzare la pipeline front-end {#update-project-enable-frontend-pipeline}

In questo capitolo, verranno apportate modifiche alla configurazione di __Progetto WKND Sites__ per utilizzare la pipeline front-end per distribuire JavaScript e CSS, anziché richiedere un’esecuzione completa della pipeline full stack. Questo separa lo sviluppo e il ciclo di vita di implementazione degli artefatti front-end e back-end, consentendo un processo di sviluppo più rapido e iterativo complessivo.

## Obiettivi {#objectives}

* Aggiornare il progetto full stack per utilizzare la pipeline front-end

## Panoramica delle modifiche alla configurazione nel progetto AEM full stack

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Prerequisiti {#prerequisites}

Questa è un&#39;esercitazione in più parti e si presume che abbiate rivisto [Modulo &#39;ui.frontend&#39;](./review-uifrontend-module.md).


## Modifiche al progetto full stack AEM

Per l’esecuzione di un test, sono disponibili tre modifiche di configurazione relative al progetto e una modifica di stile da distribuire, per un totale quindi di quattro modifiche specifiche nel progetto WKND per abilitarlo per il contratto della pipeline front-end.

1. Rimuovi il `ui.frontend` modulo dal ciclo di sviluppo full stack

   * In, la directory principale del progetto WKND Sites `pom.xml` commenta `<module>ui.frontend</module>` voce modulo secondario.

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

   * E la dipendenza relativa al commento da `ui.apps/pom.xml`

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

1. Prepara il `ui.frontend` per il contratto della pipeline front-end aggiungendo due nuovi file di configurazione webpack.

   * Copia il `webpack.common.js` as `webpack.theme.common.js`, e modifica `output` proprietà e `MiniCssExtractPlugin`, `CopyWebpackPlugin` parametri di configurazione del plug-in:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Copia il `webpack.prod.js` as `webpack.theme.prod.js`, e modificare il `common` posizione della variabile nel file sopra indicato come

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >Le due modifiche alla configurazione &quot;webpack&quot; di cui sopra devono avere nomi di file di output e cartelle diversi, in modo da poter facilmente distinguere tra gli artefatti front-end della pipeline clientlib (full-stack) e quelli generati dal tema (front-end).
   >
   >Come hai indovinato, le modifiche di cui sopra possono essere ignorate per utilizzare anche le configurazioni esistenti del webpack, ma sono necessarie le modifiche di seguito.
   >
   >Sta a te nominarli o organizzarli.


   * In `package.json` file, assicurati che il  `name` il valore della proprietà è uguale al nome del sito dalla proprietà `/conf` nodo. E sotto `scripts` proprietà, a `build` script che illustra come creare i file front-end da questo modulo.

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

1. Prepara il `ui.content` per la pipeline front-end aggiungendo due configurazioni Sling.

   * Crea un file in `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - questo include tutti i file front-end che `ui.frontend` genera un modulo sotto `dist` mediante il processo di creazione webpack.

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
   >    Vedi la sezione completa [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) nel __Progetto AEM WKND Sites__.


   * Secondo `com.adobe.aem.wcm.site.manager.config.SiteConfig` con `themePackageName` valore uguale al valore `package.json` e `name` valore della proprietà e `siteTemplatePath` che punta a un `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` valore del percorso stub.

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
   >    Vedi, la sezione completa [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) nel __Progetto AEM WKND Sites__.

1. Stiamo modificando un tema o degli stili da distribuire tramite pipeline front-end per un’esecuzione di test `text-color` per Adobe rosso (o selezionarne uno tuo) aggiornando il `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Infine, invia queste modifiche all’archivio Git di Adobe del programma.


>[!AVAILABILITY]
>
> Queste modifiche sono disponibili su GitHub all&#39;interno del [__pipeline front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) ramo del __Progetto AEM WKND Sites__.


## Attenzione - _Abilita pipeline front-end_ pulsante

Il [Selettore della barra](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) di [Sito](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) mostra il **Abilita pipeline front-end** dopo aver selezionato la directory principale del sito o la pagina del sito. Clic **Abilita pipeline front-end** sovrascriverà quanto sopra **Configurazioni Sling**, assicurati che **non si fa clic su** questo pulsante dopo aver distribuito le modifiche di cui sopra tramite l’esecuzione della pipeline di Cloud Manager.

![Pulsante Abilita pipeline front-end](assets/enable-front-end-Pipeline-button.png)

Se fai clic su di esso per errore, devi eseguire nuovamente le pipeline per assicurarti che vengano ripristinati il contratto e le modifiche della pipeline front-end.

## Congratulazioni. {#congratulations}

Congratulazioni, hai aggiornato il progetto WKND Sites per abilitarlo per il contratto della pipeline front-end.

## Passaggi successivi {#next-steps}

Nel prossimo capitolo, [Distribuire utilizzando la pipeline front-end](create-frontend-pipeline.md), creerai ed eseguirai una pipeline front-end e verificherai come __spostato__ dalla distribuzione delle risorse front-end basata su &quot;/etc.clientlibs&quot;.
