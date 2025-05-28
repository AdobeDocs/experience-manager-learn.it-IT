---
title: Aggiorna il progetto AEM full stack per utilizzare la pipeline front-end
description: Scopri come aggiornare il progetto AEM full stack per abilitarlo per la pipeline front-end, in modo da creare e distribuire solo gli artefatti front-end.
version: Experience Manager as a Cloud Service
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
source-git-commit: b395b3b84e63fe6c24e597d1628f4aed5ba47469
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# Aggiorna il progetto AEM full stack per utilizzare la pipeline front-end {#update-project-enable-frontend-pipeline}

In questo capitolo vengono apportate modifiche di configurazione al progetto __WKND Sites__ per utilizzare la pipeline front-end per distribuire JavaScript e CSS, anziché richiedere un&#39;esecuzione completa della pipeline full stack. Questo separa lo sviluppo e il ciclo di vita di implementazione degli artefatti front-end e back-end, consentendo un processo di sviluppo più rapido e iterativo complessivo.

## Obiettivi {#objectives}

* Aggiornare il progetto full stack per utilizzare la pipeline front-end

## Panoramica delle modifiche alla configurazione nel progetto AEM full stack

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Prerequisiti {#prerequisites}

Questo è un tutorial in più parti e si presume che tu abbia rivisto il modulo [&#39;ui.frontend&#39;](./review-uifrontend-module.md).


## Modifiche al progetto full stack di AEM

Per l’esecuzione di un test, sono disponibili tre modifiche di configurazione relative al progetto e una modifica di stile da distribuire, per un totale quindi di quattro modifiche specifiche nel progetto WKND per abilitarlo per il contratto della pipeline front-end.

1. Rimuovi il modulo `ui.frontend` dal ciclo di compilazione full stack

   * In, la radice del progetto WKND Sites `pom.xml` ha aggiunto un commento alla voce del sottomodulo `<module>ui.frontend</module>`.

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

   * Dipendenza correlata al commento da `ui.apps/pom.xml`

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

1. Prepara il modulo `ui.frontend` per il contratto della pipeline front-end aggiungendo due nuovi file di configurazione webpack.

   * Copiare `webpack.common.js` esistente come `webpack.theme.common.js` e modificare la proprietà `output` e i parametri di configurazione del plug-in `MiniCssExtractPlugin`, `CopyWebpackPlugin` come indicato di seguito:

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
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './theme' }
           ]
       })
   ...
   ```

   * Copiare `webpack.prod.js` esistente come `webpack.theme.prod.js` e modificare la posizione della variabile `common` nel file precedente come

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


   * Nel file `package.json`, assicurarsi che il valore della proprietà `name` sia uguale al nome del sito del nodo `/conf`. E sotto la proprietà `scripts`, uno script `build` che indica come generare i file front-end da questo modulo.

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

1. Prepara il modulo `ui.content` per la pipeline front-end aggiungendo due configurazioni Sling.

   * Crea un file in `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`. Sono inclusi tutti i file front-end generati dal modulo `ui.frontend` nella cartella `dist` tramite il processo di compilazione Webpack.

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
   >    Vedi [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) completo nel __progetto AEM WKND Sites__.


   * Secondi `com.adobe.aem.wcm.site.manager.config.SiteConfig` con il valore `themePackageName` uguale al valore della proprietà `package.json` e `name` e `siteTemplatePath` che punta a un valore del percorso stub `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0`.

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
   >    Vedi [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) completo nel __progetto AEM WKND Sites__.

1. Uno o più stili cambiano per la distribuzione tramite pipeline front-end per un&#39;esecuzione dei test. Stiamo cambiando `text-color` in rosso Adobe (oppure puoi sceglierne uno tuo) aggiornando `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Infine, invia queste modifiche all’archivio Git Adobe del programma.


>[!AVAILABILITY]
>
> Queste modifiche sono disponibili su GitHub all&#39;interno della [__pipeline front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) del ramo __progetto AEM WKND Sites__.


## Attenzione: _pulsante Abilita pipeline front-end_

L&#39;opzione [Sito](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) del selettore della barra](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) mostra il pulsante **Abilita pipeline front-end** quando selezioni la directory principale del sito o la pagina del sito. [ Facendo clic sul pulsante **Abilita pipeline front-end**, verranno ignorate le **configurazioni Sling precedenti**. Assicurarsi che **non si faccia clic su questo pulsante** dopo aver distribuito le modifiche precedenti tramite l&#39;esecuzione della pipeline Cloud Manager.

![Pulsante Abilita pipeline front-end](assets/enable-front-end-Pipeline-button.png)

Se fai clic su di esso per errore, devi eseguire nuovamente le pipeline per assicurarti che vengano ripristinati il contratto e le modifiche della pipeline front-end.

## Congratulazioni. {#congratulations}

Congratulazioni, hai aggiornato il progetto WKND Sites per abilitarlo per il contratto della pipeline front-end.

## Passaggi successivi {#next-steps}

Nel prossimo capitolo, [Distribuisci utilizzando la pipeline front-end](create-frontend-pipeline.md), creerai ed eseguirai una pipeline front-end e verificherai come __ci siamo allontanati__ dalla distribuzione di risorse front-end basata su &#39;/etc.clientlibs&#39;.
