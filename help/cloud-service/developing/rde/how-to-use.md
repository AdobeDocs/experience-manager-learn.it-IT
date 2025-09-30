---
title: Come utilizzare l’ambiente di sviluppo rapido
description: Scopri come utilizzare l’ambiente di sviluppo rapido per implementare codice e contenuti dal computer locale.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 792
source-git-commit: 2f7e10680c7211da836e33fdd241cd7f5d633d5f
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 1%

---

# Come utilizzare l’ambiente di sviluppo rapido

Scopri **come utilizzare** l&#39;ambiente di sviluppo rapido (RDE) in AEM as a Cloud Service. Distribuisci codice e contenuti per velocizzare i cicli di sviluppo del codice quasi finale nell’RDE, dall’ambiente di sviluppo integrato (IDE) preferito.

Utilizzando il [progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) imparerai a distribuire vari artefatti AEM nell&#39;RDE eseguendo il comando `install` di AEM-RDE dall&#39;IDE preferito.

- Distribuzione di pacchetti di codice e contenuti di AEM (tutti, ui.apps)
- Distribuzione del bundle OSGi e del file di configurazione
- Distribuzione delle configurazioni di Apache e Dispatcher come file zip
- Singoli file come HTL, `.content.xml` (finestra di dialogo XML) distribuzione
- Rivedi altri comandi RDE come `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Prerequisito

Clona il progetto [WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) e aprilo nell&#39;IDE preferito per distribuire gli artefatti AEM nell&#39;RDE.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Quindi, generalo e implementalo nell’AEM-SDK locale eseguendo il seguente comando maven.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Distribuire gli artefatti di AEM utilizzando il plug-in AEM-RDE

Verificare innanzitutto che sia installato il [più recente modulo CLI `aio`](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools#aio-cli).

Quindi, utilizzare il comando `aio aem:rde:install` per distribuire vari artefatti di AEM.

### Distribuisci `all` e `dispatcher` pacchetti

Un punto di partenza comune è la distribuzione dei pacchetti `all` e `dispatcher` mediante l&#39;esecuzione dei seguenti comandi.

```shell
# Install the 'all' content package (zip file)
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' deployment artifact (zip file)
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

In caso di distribuzioni corrette, verifica il sito WKND sia nei servizi di authoring che in quelli di pubblicazione. Dovresti essere in grado di aggiungere, modificare e pubblicare il contenuto delle pagine del sito WKND.

### Migliorare e distribuire un componente

Miglioriamo `Hello World Component` e implementiamolo nell&#39;RDE.

1. Apri il file XML della finestra di dialogo (`.content.xml`) dalla cartella `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/`
1. Aggiungi il campo di testo `Description` dopo il campo di dialogo `Text` esistente

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Apri il file `helloworld.html` dalla cartella `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld`
1. Eseguire il rendering della proprietà `Description` dopo l&#39;elemento `<div>` esistente della proprietà `Text`.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Verifica le modifiche sul SDK AEM locale eseguendo la build Maven o sincronizzando singoli file.

1. Distribuire le modifiche all&#39;RDE tramite il pacchetto `ui.apps` o distribuendo i singoli file Dialog e HTL:

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

1. Verificare le modifiche nell&#39;RDE aggiungendo o modificando `Hello World Component` in una pagina del sito WKND.

### Rivedi le opzioni del comando `install`

Nell&#39;esempio di comando di distribuzione di singoli file sopra riportato, i flag `-t` e `-p` vengono utilizzati rispettivamente per indicare il tipo e la destinazione del percorso JCR. Esaminiamo le opzioni di comando `install` disponibili eseguendo il comando seguente.

```shell
$ aio aem:rde:install --help
```

I flag sono auto-esplicativi. Il flag `-s` è utile per indirizzare la distribuzione solo ai servizi di authoring o pubblicazione. Utilizza il flag `-t` quando distribuisci i file **content-file o content-xml** insieme al flag `-p` per specificare il percorso JCR di destinazione nell&#39;ambiente AEM RDE.

### Distribuire il bundle OSGi

Per informazioni su come distribuire il bundle OSGi, miglioriamo la classe Java™ `HelloWorldModel` e distribuiamola all&#39;RDE.

1. Apri il file `HelloWorldModel.java` dalla cartella `core/src/main/java/com/adobe/aem/guides/wknd/core/models`
1. Aggiorna il metodo `init()` come segue:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Verifica le modifiche su AEM-SDK locale distribuendo il bundle `core` tramite il comando maven
1. Distribuire le modifiche all&#39;RDE eseguendo il comando seguente

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Verificare le modifiche nell&#39;RDE aggiungendo o modificando `Hello World Component` in una pagina del sito WKND.

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
>Per installare una configurazione OSGi solo in un&#39;istanza Autore o Publish, utilizza il flag `-s`.


### Distribuire la configurazione di Apache o Dispatcher

Impossibile distribuire singolarmente i file di configurazione Apache o Dispatcher **&#x200B;**, ma è necessario distribuire l&#39;intera struttura di cartelle Dispatcher sotto forma di file ZIP.

1. Apportare la modifica desiderata nel file di configurazione del modulo `dispatcher`. A scopo dimostrativo, aggiornare `dispatcher/src/conf.d/available_vhosts/wknd.vhost` in modo da memorizzare nella cache i file `html` solo per 60 secondi.

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

1. Verificare le modifiche localmente. Per ulteriori dettagli, vedere [Eseguire Dispatcher localmente](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools).
1. Distribuire le modifiche apportate all&#39;RDE eseguendo il comando seguente:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verificare le modifiche nell&#39;RDE.

### Distribuisci file di configurazione (YAML)

I file di configurazione CDN, delle attività di manutenzione, dell&#39;inoltro del registro e dell&#39;autenticazione API AEM possono essere distribuiti in RDE utilizzando il comando `install`. Queste configurazioni vengono gestite come file YAML nella cartella `config` del progetto AEM. Per ulteriori dettagli, vedi [Configurazioni supportate](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/operations/config-pipeline#configurations).

Per informazioni su come distribuire i file di configurazione, è possibile migliorare il file di configurazione `cdn` e distribuirlo in RDE.

1. Apri il file `cdn.yaml` dalla cartella `config`
1. Aggiorna la configurazione desiderata, ad esempio il limite di velocità a 200 richieste al secondo

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     trafficFilters:
       rules:
       #  Block client for 5m when it exceeds an average of 100 req/sec to origin on a time window of 10sec
       - name: limit-origin-requests-client-ip
         when:
           reqProperty: tier
           equals: 'publish'
         rateLimit:
           limit: 200 # updated rate limit
           window: 10
           count: fetches
           penalty: 300
           groupBy:
             - reqProperty: clientIp
         action: log
   ...
   ```

1. Distribuire le modifiche all&#39;RDE eseguendo il comando seguente

   ```shell
   $ aio aem:rde:install -t env-config ./config
   ```

1. Verificare le modifiche nell&#39;RDE


## Comandi aggiuntivi del plug-in AEM RDE

Esaminiamo i comandi aggiuntivi del plug-in RDE di AEM per gestire e interagire con l’RDE dal computer locale.

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

Utilizzando i comandi di cui sopra, l&#39;RDE può essere gestito dall&#39;IDE preferito per un ciclo di sviluppo/implementazione più rapido.

## Passaggio successivo

Scopri il ciclo di vita di [sviluppo/distribuzione utilizzando RDE](./development-life-cycle.md) per distribuire funzionalità in modo rapido.


## Risorse aggiuntive

[Documentazione sui comandi RDE](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments)

[Plug-in CLI di Adobe I/O Runtime per interazioni con ambienti di sviluppo rapido AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configurazione del progetto AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup)
