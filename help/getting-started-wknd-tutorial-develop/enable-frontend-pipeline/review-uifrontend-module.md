---
title: Rivedi il modulo ui.frontend del progetto full stack
description: Rivedi il ciclo di sviluppo, distribuzione e durata di consegna front-end di un progetto AEM Sites full-stack basato su Maven.
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
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Rivedi il modulo &quot;ui.frontend&quot; del progetto AEM full-stack {#aem-full-stack-ui-frontent}

In questo capitolo esaminiamo lo sviluppo, la distribuzione e la distribuzione di artefatti front-end di un progetto AEM full-stack, concentrandoci sul modulo &quot;ui.frontend&quot; del __Progetto WKND Sites__.


## Obiettivi {#objective}

* Comprendere la generazione e il flusso di implementazione di artefatti front-end in un progetto full-stack dell’AEM
* Rivedi il progetto full stack dell’AEM `ui.frontend` del modulo [webpack](https://webpack.js.org/) configura
* Processo di generazione della libreria client AEM (nota anche come clientlibs)

## Flusso di distribuzione front-end per progetti AEM full-stack e Creazione rapida di siti

>[!IMPORTANT]
>
>Questo video spiega e illustra il flusso front-end per entrambi **Creazione full stack e rapida di siti** progetti per delineare la sottile differenza nel modello di creazione, distribuzione e consegna delle risorse front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Prerequisiti {#prerequisites}


* Clona il [Progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd)
* Ha creato e implementato il progetto WKND Sites dell’AEM clonato in AEM as a Cloud Service.

Vedi il progetto del sito WKND dell’AEM [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) per ulteriori dettagli.

## Flusso degli artefatti front-end del progetto full-stack dell’AEM {#flow-of-frontend-artifacts}

Di seguito è riportata una rappresentazione di alto livello del __sviluppo, distribuzione e consegna__ flusso degli artefatti front-end in un progetto AEM full-stack.

![Sviluppo, distribuzione e distribuzione di artefatti front-end](assets/Dev-Deploy-Delivery-AEM-Project.png)


Durante la fase di sviluppo, le modifiche front-end come lo stile e il rebranding vengono eseguite aggiornando i file CSS e JS dalla sezione `ui.frontend/src/main/webpack` cartella. Quindi durante la generazione, il [webpack](https://webpack.js.org/) module-bundler e il plug-in maven trasformano questi file in librerie client AEM ottimizzate in `ui.apps` modulo.

Le modifiche front-end vengono implementate nell’ambiente as a Cloud Service AEM durante l’esecuzione del [__Full stack__ pipeline in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

Le risorse front-end vengono distribuite ai browser web tramite percorsi URI che iniziano con `/etc.clientlibs/`, e sono in genere memorizzati nella cache in Dispatcher per AEM e CDN.


>[!NOTE]
>
> Analogamente, nella __Percorso di creazione rapida di siti AEM__, il [modifiche front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) vengono implementati nell’ambiente AEM as a Cloud Service eseguendo il comando __Front-end__ pipeline, vedi [Configurare la pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Verifica configurazioni webpack nel progetto WKND Sites {#development-frontend-webpack-clientlib}

* Sono tre __webpack__ file di configurazione utilizzati per raggruppare le risorse front-end dei siti WKND.

   1. `webpack.common` - Contiene __comune__ per istruire il bundling e l’ottimizzazione delle risorse WKND. Il __output__ indica dove emettere i file consolidati (noti anche come bundle JavaScript, ma da non confondere con i bundle OSGi dell’AEM) che crea. Il nome predefinito è impostato su `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` contiene __sviluppo__ configurazione per webpack-dev-server e punta al modello HTML da utilizzare. Contiene anche una configurazione proxy a un’istanza AEM in esecuzione su `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` contiene __produzione__ e utilizza i plug-in per trasformare i file di sviluppo in bundle ottimizzati.

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


* Le risorse in bundle vengono spostate in `ui.apps` modulo che utilizza [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) , utilizzando la configurazione gestita in `clientlib.config.js` file.

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

* Il __frontend-maven-plugin__ da `ui.frontend/pom.xml` orchestra il bundling webpack e la generazione clientlib durante la creazione del progetto AEM.

`$ mvn clean install -PautoInstallSinglePackage`

### Distribuzione in AEM as a Cloud Service {#deployment-frontend-aemaacs}

Il [__Full stack__ pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) implementa queste modifiche in un ambiente AEM as a Cloud Service.


### Consegna da AEM as a Cloud Service {#delivery-frontend-aemaacs}

Le risorse front-end distribuite tramite la pipeline full stack vengono distribuite dal sito AEM ai browser web come `/etc.clientlibs` file. Per verificarlo, visita il [sito WKND ospitato pubblicamente](https://wknd.site/content/wknd/us/en.html) e la visualizzazione dell&#39;origine della pagina Web.

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

Nel prossimo capitolo, [Aggiorna progetto per utilizzare la pipeline front-end](update-project.md), aggiornerai il progetto WKND Sites dell’AEM per abilitarlo per il contratto della pipeline front-end.
