---
title: Rivedi il modulo ui.frontend del progetto full stack
description: Rivedi il ciclo di sviluppo, distribuzione e durata di consegna front-end di un progetto AEM Sites full-stack basato su Maven.
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
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Rivedi il modulo &quot;ui.frontend&quot; del progetto AEM full-stack {#aem-full-stack-ui-frontent}

In questo capitolo esaminiamo lo sviluppo, la distribuzione e la distribuzione di artefatti front-end di un progetto AEM full-stack, concentrandoci sul modulo &quot;ui.frontend&quot; del __progetto Sites WKND__.


## Obiettivi {#objective}

* Comprendere la generazione e il flusso di distribuzione di artefatti front-end in un progetto full-stack di AEM
* Verifica le configurazioni [webpack](https://webpack.js.org/) del modulo `ui.frontend` del progetto full stack di AEM
* Processo di generazione della libreria client di AEM (nota anche come clientlibs)

## Flusso di distribuzione front-end per progetti AEM full-stack e Creazione rapida di siti

>[!IMPORTANT]
>
>Questo video illustra e illustra il flusso front-end per i progetti **Full-Stack e Creazione rapida di siti** per delineare la sottile differenza nel modello di creazione, distribuzione e consegna delle risorse front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Prerequisiti {#prerequisites}


* Clona il [progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd)
* Genera e implementa in AEM as a Cloud Service il progetto clonato AEM WKND Sites.

Per ulteriori dettagli, vedi il progetto del sito WKND AEM [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md).

## Flusso di artefatti front-end per progetto full-stack di AEM {#flow-of-frontend-artifacts}

Di seguito è riportata una rappresentazione di alto livello del flusso __sviluppo, distribuzione e consegna__ degli artefatti front-end in un progetto AEM full-stack.

![Sviluppo, distribuzione e distribuzione di artifact front-end](assets/Dev-Deploy-Delivery-AEM-Project.png)


Durante la fase di sviluppo, le modifiche front-end come lo stile e il rebranding vengono eseguite aggiornando i file CSS e JS dalla cartella `ui.frontend/src/main/webpack`. Quindi, durante la fase di creazione, il modulo-bundler [webpack](https://webpack.js.org/) e il plug-in maven trasformano questi file in clientlibs ottimizzati di AEM nel modulo `ui.apps`.

Le modifiche front-end vengono distribuite nell&#39;ambiente AEM as a Cloud Service durante l&#39;esecuzione della pipeline [__Full-stack__ in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

Le risorse front-end vengono distribuite ai browser web tramite percorsi URI che iniziano con `/etc.clientlibs/` e sono in genere memorizzate nella cache in AEM Dispatcher e CDN.


>[!NOTE]
>
> Analogamente, nel __Percorso di Creazione Rapida dei Siti AEM__, le [modifiche front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) vengono distribuite nell&#39;ambiente AEM as a Cloud Service eseguendo la pipeline __front-end__. Vedere [Configurazione della pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Verifica configurazioni webpack nel progetto WKND Sites {#development-frontend-webpack-clientlib}

* Sono disponibili tre file di configurazione __webpack__ utilizzati per raggruppare le risorse front-end dei siti WKND.

   1. `webpack.common` - Contiene la configurazione __common__ per istruire il bundling e l&#39;ottimizzazione delle risorse WKND. La proprietà __output__ indica dove emettere i file consolidati (noti anche come bundle di JavaScript, ma da non confondere con i bundle OSGi di AEM) che crea. Il nome predefinito è `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` contiene la configurazione __development__ per webpack-dev-server e punta al modello HTML da utilizzare. Contiene anche una configurazione proxy a un&#39;istanza AEM in esecuzione su `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` contiene la configurazione __production__ e utilizza i plug-in per trasformare i file di sviluppo in bundle ottimizzati.

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* Le risorse in bundle vengono spostate nel modulo `ui.apps` utilizzando il plug-in [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator), utilizzando la configurazione gestita nel file `clientlib.config.js`.

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* __frontend-maven-plugin__ da `ui.frontend/pom.xml` orchestra la generazione di webpack bundling e clientlib durante la generazione del progetto AEM.

`$ mvn clean install -PautoInstallSinglePackage`

### Implementazione in AEM as a Cloud Service {#deployment-frontend-aemaacs}

La pipeline [__full stack__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) distribuisce queste modifiche in un ambiente AEM as a Cloud Service.


### Consegna da AEM as a Cloud Service {#delivery-frontend-aemaacs}

Le risorse front-end distribuite tramite la pipeline full-stack vengono distribuite dal sito AEM ai browser Web come `/etc.clientlibs` file. Puoi verificare questo fatto visitando il [sito WKND ospitato pubblicamente](https://wknd.site/content/wknd/us/en.html) e visualizzando l&#39;origine della pagina Web.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Congratulazioni. {#congratulations}

Congratulazioni, hai esaminato il modulo ui.frontend del progetto full stack

## Passaggi successivi {#next-steps}

Nel prossimo capitolo, [Aggiorna progetto per utilizzare la pipeline front-end](update-project.md), aggiornerai il progetto AEM WKND Sites per abilitarlo per il contratto della pipeline front-end.
