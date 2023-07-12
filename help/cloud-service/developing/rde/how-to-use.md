---
title: Come utilizzare l’ambiente di sviluppo rapido
description: Scopri come utilizzare l’ambiente di sviluppo rapido per implementare codice e contenuti dal computer locale.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
source-git-commit: 27d065761643030de68176ebb4ca10bc152844df
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---

# Come utilizzare l’ambiente di sviluppo rapido

Scopri **come utilizzare** Rapid Development Environment (RDE) nell’AEM as a Cloud Service. Distribuisci codice e contenuti per velocizzare i cicli di sviluppo del codice quasi finale nell’RDE, dall’ambiente di sviluppo integrato (IDE) preferito.

Utilizzo di [Progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) scopri come implementare vari artefatti dell’AEM AEM nell’RDE eseguendo gli `install` comando dall&#39;IDE preferito.

- Implementazione del pacchetto di codice e contenuti AEM (all, ui.apps)
- Distribuzione del bundle OSGi e del file di configurazione
- Apache e Dispatcher configurano la distribuzione come file zip
- Singoli file come HTL, `.content.xml` Distribuzione (finestra di dialogo XML)
- Rivedi altri comandi RDE come `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Prerequisito

Clona il [Siti WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) proiettarlo e aprirlo nell’IDE preferito per distribuire gli artefatti AEM sull’RDE.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Quindi, generalo e implementalo nell’AEM-SDK locale eseguendo il seguente comando maven.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Distribuire gli artefatti AEM utilizzando il plug-in AEM-RDE

Utilizzo di `aem:rde:install` , distribuiamo vari artefatti AEM.

### Distribuisci `all` e `dispatcher` pacchetti

Un punto di partenza comune è la prima implementazione di `all` e `dispatcher` mediante l&#39;esecuzione dei seguenti comandi.

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

In caso di distribuzioni corrette, verifica il sito WKND sia nell’istanza di authoring che in quella di pubblicazione. Dovresti essere in grado di aggiungere, modificare e pubblicare il contenuto delle pagine del sito WKND.

### Migliorare e distribuire un componente

Miglioriamo la `Hello World Component` e distribuirlo nell&#39;RDE.

1. Apri la finestra di dialogo XML (`.content.xml`) file da `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` cartella
1. Aggiungi il `Description` campo di testo dopo il `Text` campo finestra di dialogo

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Apri `helloworld.html` file da `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` cartella
1. Rendering `Description` proprietà dopo il `<div>` elemento del `Text` proprietà.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Verifica le modifiche su AEM-SDK locale eseguendo la build Maven o sincronizzando i singoli file.

1. Distribuire le modifiche all&#39;RDE tramite `ui.apps` o distribuendo i singoli file Dialog e HTL.

   ```shell
   # Using 'ui.apps' package
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Verificare le modifiche apportate all&#39;RDE aggiungendo o modificando `Hello World Component` in una pagina del sito WKND.

### Rivedi `install` opzioni di comando

Nell&#39;esempio di comando di distribuzione di singoli file sopra riportato, il `-t` e `-p` I flag vengono utilizzati rispettivamente per indicare il tipo e la destinazione del percorso JCR. Esaminiamo le `install` opzioni di comando eseguendo il comando seguente.

```shell
$ aio aem:rde:install --help
```

I contrassegni sono auto-esplicativi, `-s` Questo flag è utile per indirizzare la distribuzione solo ai servizi di authoring o pubblicazione. Utilizza il `-t` durante la distribuzione di **content-file o content-xml** file insieme a `-p` flag per specificare il percorso JCR di destinazione nell’ambiente AEM RDE.

### Distribuire il bundle OSGi

Per scoprire come distribuire il bundle OSGi, miglioriamo il `HelloWorldModel` Java™ e distribuirla all&#39;RDE.

1. Apri `HelloWorldModel.java` file da `core/src/main/java/com/adobe/aem/guides/wknd/core/models` cartella
1. Aggiornare il `init()` metodo come segue:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Verificare le modifiche sull’AEM-SDK locale implementando `core` bundle tramite il comando maven
1. Distribuire le modifiche all&#39;RDE eseguendo il comando seguente

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Verificare le modifiche apportate all&#39;RDE aggiungendo o modificando `Hello World Component` in una pagina del sito WKND.

### Distribuire la configurazione OSGi

Puoi distribuire i singoli file di configurazione o il pacchetto di configurazione completo, ad esempio:

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>Per installare una configurazione OSGi solo su un’istanza Autore o Publish, utilizza `-s` flag.


### Distribuire la configurazione di Apache o Dispatcher

File di configurazione di Apache o Dispatcher **impossibile distribuire singolarmente**, ma l’intera struttura di cartelle di Dispatcher deve essere distribuita sotto forma di un file ZIP.

1. Apportare la modifica desiderata nel file di configurazione del `dispatcher` a scopo dimostrativo, aggiorna il `dispatcher/src/conf.d/available_vhosts/wknd.vhost` per memorizzare nella cache `html` solo per 60 secondi.

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. Verifica le modifiche localmente, vedi [Eseguire Dispatcher localmente](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) per ulteriori dettagli.
1. Distribuire le modifiche apportate all&#39;RDE eseguendo il comando seguente:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verificare le modifiche nell&#39;RDE

## Comandi aggiuntivi del plug-in AEM RDE

Esaminiamo i comandi aggiuntivi del plug-in AEM RDE per gestire e interagire con l’RDE dal computer locale.

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

Utilizzando i comandi di cui sopra, l&#39;RDE può essere gestito dall&#39;IDE preferito per velocizzare il ciclo di sviluppo/implementazione.

## Passaggio successivo

Scopri di più su [ciclo di vita di sviluppo/implementazione tramite RDE](./development-life-cycle.md) per offrire funzioni in modo rapido.


## Risorse aggiuntive

[Documentazione sui comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Plug-in Adobe I/O Runtime CLI per interazioni con ambienti di sviluppo rapido AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configurazione del progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
