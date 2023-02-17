---
title: Come utilizzare l'ambiente di sviluppo rapido
description: Scopri come utilizzare l’ambiente di sviluppo rapido per distribuire codice e contenuti dal computer locale.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 0%

---


# Come utilizzare l&#39;ambiente di sviluppo rapido

Scopri **come utilizzare** Ambiente di sviluppo rapido (RDE) in AEM as a Cloud Service. Distribuisci codice e contenuti per cicli di sviluppo più rapidi del codice quasi finale all&#39;RDE, dal tuo ambiente di sviluppo integrato (IDE) preferito.

Utilizzo [AEM progetto Siti WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) scopri come distribuire vari artefatti AEM all’RDE eseguendo AEM-RDE `install` comando dal tuo IDE preferito.

- Distribuzione di codice AEM e pacchetti di contenuti (tutto, ui.apps)
- Distribuzione di file di configurazione e bundle OSGi
- Apache e Dispatcher configurano la distribuzione come file zip
- File individuali come HTL, `.content.xml` Distribuzione (finestra di dialogo XML)
- Rivedi altri comandi RDE come `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## Prerequisito

Clona il [Siti WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) proiettare e aprirlo nel tuo IDE preferito per distribuire gli artefatti AEM sull’RDE.

    &quot;guscio
    $ clone git git@github.com:adobe/aem-guides-wknd.git
    &quot;

Quindi, crealo e distribuiscilo nell&#39;SDK AEM locale eseguendo il seguente comando maven.

    &quot;
    $ cd aem-guides-wknd/
    $ mvn clean install -PautoInstallSinglePackage
    &quot;

## Distribuire artefatti AEM utilizzando il plug-in AEM-RDE

Utilizzo della `aem:rde:install` comando, distribuiamo vari artefatti AEM.

### Distribuzione `all` e `dispatcher` pacchetti

Un punto di partenza comune è quello di implementare prima il `all` e `dispatcher` esegue i seguenti comandi.

    &quot;guscio
    # Installare il pacchetto &#39;all&#39;
    $ aio aem:rde:installa all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip
    
    # Installare lo zip &#39;dispatcher&#39;
    $ aio aem:rde:installa dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
    &quot;

Dopo le distribuzioni corrette, verifica il sito WKND sia sui servizi di authoring che di pubblicazione. Dovresti essere in grado di aggiungere, modificare il contenuto sulle pagine del sito WKND e pubblicarlo.

### Migliorare e distribuire un componente

Miglioriamo il `Hello World Component` e distribuirlo all&#39;RDE.

1. Apri la finestra di dialogo XML (`.content.xml`) da `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` cartella
1. Aggiungi il `Description` campo di testo dopo quello esistente `Text` campo di dialogo

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
1. Rendering `Description` dopo l&#39;esistente `<div>` elemento `Text` proprietà.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Verifica le modifiche sull’SDK AEM locale eseguendo la build maven o sincronizzando i singoli file.

1. Implementare le modifiche all’RDE tramite `ui.apps` o distribuendo i singoli file Dialog e HTL.

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

1. Verifica le modifiche all’RDE aggiungendo o modificando il `Hello World Component` su una pagina del sito WKND.

### Consulta la sezione `install` opzioni di comando

Nell&#39;esempio di comando di distribuzione dei singoli file riportato sopra, la `-t` e `-p` i flag vengono utilizzati per indicare rispettivamente il tipo e la destinazione del percorso JCR. Esaminiamo i dati disponibili `install` opzioni di comando eseguendo il comando seguente.

    &quot;guscio
    $ aio aem:rde:install —help
    &quot;

Le bandiere sono auto-esplicative, `-s` Questo flag è utile per eseguire il targeting della distribuzione solo per i servizi di authoring o pubblicazione. Utilizza la `-t` flag durante la distribuzione di **content-file o content-xml** insieme ai `-p` flag per specificare il percorso JCR di destinazione nell’ambiente RDE AEM.

### Distribuzione del bundle OSGi

Per scoprire come distribuire il bundle OSGi, miglioriamo il `HelloWorldModel` Classe Java™ e implementalo nell&#39;RDE.

1. Apri `HelloWorldModel.java` file da `core/src/main/java/com/adobe/aem/guides/wknd/core/models` cartella
1. Aggiorna `init()` come segue:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Verifica le modifiche nell’SDK AEM locale distribuendo il `core` bundle tramite comando maven
1. Distribuire le modifiche all&#39;RDE eseguendo il seguente comando

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Verifica le modifiche all’RDE aggiungendo o modificando il `Hello World Component` su una pagina del sito WKND.

### Distribuzione della configurazione OSGi

Puoi distribuire i singoli file di configurazione o il pacchetto di configurazione completo, ad esempio:

    &quot;guscio
    # Distribuzione di un singolo file di configurazione
    $ aio aem:rde:installa ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json
    
    # O distribuire il pacchetto di configurazione completo
    $ cd ui.config
    $ mvn pacchetto pulito
    $ aio aem:rde:installa target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
    &quot;

>[!TIP]
>
>Per installare una configurazione OSGi solo su un&#39;istanza di authoring o pubblicazione, utilizza la `-s` bandiera.


### Implementare la configurazione di Apache o Dispatcher

File di configurazione di Apache o Dispatcher **non può essere distribuito singolarmente**, ma l’intera struttura di cartelle di Dispatcher deve essere distribuita sotto forma di un file ZIP.

1. Apporta una modifica desiderata nel file di configurazione del `dispatcher` per scopi dimostrativi, aggiorna il `dispatcher/src/conf.d/available_vhosts/wknd.vhost` per memorizzare nella cache `html` file solo per 60 secondi.

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

1. Verifica le modifiche localmente, vedi [Eseguire il Dispatcher localmente](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) per ulteriori dettagli.
1. Distribuisci le modifiche all’RDE eseguendo il seguente comando:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verifica le modifiche nell’RDE

## Comandi aggiuntivi del plug-in RDE AEM

Esaminiamo i comandi aggiuntivi del plug-in RDE AEM per gestire e interagiamo con l’RDE dal computer locale.

    &quot;guscio
    $ aio aem:rde —help
    Interagisci con gli ambienti RapidDev.
    
    UTILIZZO
    $ aio aem rde COMMAND
    
    COMANDI
    elimina i bundle e le configurazioni eliminati da aem rde dalla regola corrente.
    cronologia di amministrazione di aem Ottieni un elenco degli aggiornamenti apportati all&#39;ordine corrente.
    installazione di aem rde Installazione/aggiornamento di bundle, configurazioni e pacchetti di contenuti.
    reimpostazione dell&#39;ordine aem Reimposta l&#39;RDE
    riavvia aem rde Riavvia l&#39;autore e pubblica un RDE
    stato dell&#39;url di aem Ottieni un elenco dei bundle e delle configurazioni implementate nell&#39;ordine corrente.
    &quot;

Utilizzando i comandi di cui sopra, il tuo RDE può essere gestito dal tuo IDE preferito per un ciclo di vita di sviluppo/distribuzione più veloce.

## Passaggio successivo

Scopri le [ciclo di vita di sviluppo/distribuzione tramite RDE](./development-life-cycle.md) per fornire funzionalità con rapidità.


## Altro materiale di riferimento

[Documentazione sui comandi RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Plug-in Adobe I/O Runtime CLI per le interazioni con gli ambienti di sviluppo rapido AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configurazione AEM progetto](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
