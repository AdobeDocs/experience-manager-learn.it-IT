---
title: Configurare AEM per l’editor SPA e SPA remoto
description: È necessario un progetto AEM per configurare i requisiti di configurazione e contenuto di supporto per consentire all’Editor SPA dell’AEM di creare un SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
duration: 432
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 0%

---

# Configurare AEM per l’editor SPA

Mentre la base di codice dell’SPA è gestita al di fuori dell’AEM, è necessario un progetto AEM per configurare i requisiti di configurazione e contenuto di supporto. Questo capitolo illustra la creazione di un progetto AEM che contiene le configurazioni necessarie:

+ Proxy dei componenti core WCM AEM
+ Proxy pagina SPA remoto AEM
+ Modelli di pagina per SPA remoto AEM
+ Pagine AEM SPA remote al basale
+ Sottoprogetto per definire le mappature da SPA a URL AEM
+ Cartelle di configurazione OSGi

## Scarica il progetto di base da GitHub

Scarica il file `aem-guides-wknd-graphql` progetto da Github.com. Conterrà alcuni file di base utilizzati in questo progetto.

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## Creare un progetto AEM

Crea un progetto AEM in cui vengono gestite le configurazioni e i contenuti della linea di base. Questo progetto verrà generato all’interno del duplicato `aem-guides-wknd-graphql` del progetto `remote-spa-tutorial` cartella.

_Utilizza sempre la versione più recente di [Archetipo AEM](https://github.com/adobe/aem-project-archetype)._

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_L’ultimo comando rinomina semplicemente la cartella del progetto AEM in modo che sia chiaro che si tratta del progetto AEM e non deve essere confuso con SPA remoto__

Mentre `frontendModule="react"` è specificato, il `ui.frontend` progetto non utilizzato per il caso di utilizzo dell’SPA remoto. L’SPA è sviluppato e gestito esternamente all’AEM e utilizza l’AEM solo come contenuto API. Il `frontendModule="react"` Il flag è obbligatorio per il progetto e include  `spa-project` dipendenze Java™ dell’AEM e configurazione dei modelli di pagina per l’SPA remoto.

L’Archetipo progetto AEM genera i seguenti elementi che vengono utilizzati per configurare l’AEM per l’integrazione con l’SPA.

+ __Proxy dei componenti core WCM AEM__ a `ui.apps/src/.../apps/wknd-app/components`
+ __Proxy pagina remota SPA AEM__ a `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __Modelli di pagina AEM__ a `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Sottoprogetto per definire le mappature del contenuto__ a `ui.content/src/...`
+ __Pagine AEM SPA remote al basale__ a `ui.content/src/.../content/wknd-app`
+ __Cartelle di configurazione OSGi__ a `ui.config/src/.../apps/wknd-app/osgiconfig`

Quando viene generato il progetto AEM di base, alcune modifiche garantiscono la compatibilità dell&#39;Editor SPA con l&#39;SPA remoto.

## Rimuovi progetto ui.frontend

Poiché l&#39;SPA è un SPA remoto, supponiamo che sia sviluppato e gestito al di fuori del progetto AEM. Per evitare conflitti, rimuovere `ui.frontend` del progetto dalla distribuzione. Se il `ui.frontend` progetto non viene rimosso, due SPA, l&#39;SPA predefinito fornito nel `ui.frontend` e il Remote SPA, vengono caricati contemporaneamente nell’Editor SPA dell’AEM.

1. Apri il progetto AEM (`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`) nell’IDE
1. Apri la directory principale `pom.xml`
1. Commenta il `<module>ui.frontend</module` fuori dal `<modules>` list

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   Il `pom.xml` il file dovrebbe avere un aspetto simile a:

   ![Rimuovi il modulo ui.frontend dal POM di Reactor](./assets/aem-project/uifrontend-reactor-pom.png)

1. Apri `ui.apps/pom.xml`
1. Commenta il `<dependency>` il `<artifactId>wknd-app.ui.frontend</artifactId>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   Il `ui.apps/pom.xml` il file dovrebbe avere un aspetto simile a:

   ![Rimuovi la dipendenza ui.frontend da ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

Se il progetto AEM è stato creato prima di queste modifiche, eliminare manualmente il `ui.frontend` Libreria client generata da `ui.apps` progetto a `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## Mappatura dei contenuti AEM

Affinché l’AEM possa caricare l’SPA remoto nell’editor SPA, è necessario stabilire le mappature tra le route SPA e le pagine AEM utilizzate per aprire e creare i contenuti.

L’importanza di questa configurazione viene esaminata in seguito.

La mappatura può essere eseguita con [Mappatura Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) definito in `/etc/map`.

1. Nell’IDE, apri il file `ui.content` sottoprogetto
1. Accedi a  `src/main/content/jcr_root`
1. Creare una cartella `etc`
1. In entrata `etc`, crea una cartella `map`
1. In entrata `map`, crea una cartella `http`
1. In entrata `http`, crea un file `.content.xml` con il contenuto:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. In entrata `http` , crea una cartella `localhost_any`
1. In entrata `localhost_any`, crea un file `.content.xml` con il contenuto:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. In entrata `localhost_any` , crea una cartella `wknd-app-routes-adventure`
1. In entrata `wknd-app-routes-adventure`, crea un file `.content.xml` con il contenuto:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Aggiungi i nodi di mappatura a `ui.content/src/main/content/META-INF/vault/filter.xml` a quelli inclusi nel pacchetto AEM.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

La struttura delle cartelle e `.context.xml` i file dovrebbero avere un aspetto simile a:

![Mappatura Sling](./assets/aem-project/sling-mapping.png)

Il `filter.xml` il file dovrebbe avere un aspetto simile a:

![Mappatura Sling](./assets/aem-project/sling-mapping-filter.png)

Ora, quando il progetto AEM viene implementato, queste configurazioni vengono incluse automaticamente.

Gli effetti di mappatura Sling AEM in esecuzione su `http` e `localhost`quindi sostenere solo lo sviluppo locale. Durante l’implementazione in AEM as a Cloud Service, è necessario aggiungere mappature Sling simili a quella di destinazione `https` e i settori AEM as a Cloud Service appropriati. Per ulteriori informazioni, vedere [Documentazione di Sling Mapping](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Criteri di sicurezza per la condivisione delle risorse tra diverse origini

Quindi, configura l’AEM per proteggere il contenuto in modo che solo questo SPA possa accedere al contenuto dell’AEM. Configura [Condivisione delle risorse tra le diverse origini nell’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. Nell’IDE, apri il file `ui.config` Sottoprogetto Maven
1. Naviga `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Crea un file denominato `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Aggiungi quanto segue al file:

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

Il `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` il file dovrebbe avere un aspetto simile a:

![Configurazione CORS dell’editor SPA](./assets/aem-project/cors-configuration.png)

Gli elementi chiave della configurazione sono:

+ `alloworigin` specifica gli host autorizzati a recuperare il contenuto dall’AEM.
   + `localhost:3000` viene aggiunto per supportare l’SPA in esecuzione localmente
   + `https://external-hosted-app` funge da segnaposto da sostituire con il dominio su cui è ospitato l’SPA remoto.
+ `allowedpaths` specifica quali percorsi in AEM sono coperti da questa configurazione CORS. L’impostazione predefinita consente l’accesso a tutto il contenuto dell’AEM, tuttavia questo può essere limitato ai percorsi specifici a cui l’SPA può accedere, ad esempio: `/content/wknd-app`.

## Imposta pagina AEM come modello di pagina SPA remoto

L’Archetipo progetto AEM genera un progetto avviato per l’integrazione dell’AEM con un SPA remoto, ma richiede un piccolo ma importante adeguamento alla struttura della pagina AEM generata automaticamente. Il tipo della pagina AEM generata automaticamente deve essere modificato in __Pagina SPA remoto__, anziché un __Pagina SPA__.

1. Nell’IDE, apri il file `ui.content` sottoprogetto
1. Apri a `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Aggiorna questo `.content.xml` file con:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

Le modifiche principali sono aggiornamenti al `jcr:content` nodo:

+ `cq:template` a `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` a `wknd-app/components/remotepage`

Il `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` il file dovrebbe avere un aspetto simile a:

![Aggiornamenti .content.xml della home page](./assets/aem-project/home-content-xml.png)

Queste modifiche consentono a questa pagina, che funge da radice SPA nell&#39;AEM, di caricare l&#39;SPA remoto nell&#39;editor SPA.

>[!NOTE]
>
>Se questo progetto è stato distribuito in precedenza all’AEM, assicurati di eliminare la pagina AEM con il seguente comando: __Sites > App WKND > us > it > Home page app WKND__, come `ui.content`  il progetto è impostato su __unione__ nodi, anziché __aggiorna__.

Questa pagina può anche essere rimossa e ricreata come pagina SPA remota nell’AEM stesso, tuttavia poiché questa pagina viene creata automaticamente nell’ `ui.content` progetto è consigliabile aggiornarlo nella base di codice.

## Implementare il progetto AEM nell’SDK dell’AEM

1. Verificare che il servizio di creazione AEM sia in esecuzione sulla porta 4502
1. Dalla riga di comando, accedi alla directory principale del progetto AEM Maven
1. Utilizza Maven per distribuire il progetto al servizio di authoring dell’SDK AEM locale

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## Configurare la pagina AEM principale

Con il progetto AEM implementato, c&#39;è un ultimo passaggio per preparare l&#39;Editor SPA per caricare il nostro SPA remoto. In AEM, contrassegna la pagina AEM che corrisponde alla radice SPA,`/content/wknd-app/us/en/home`, generato dall’archetipo del progetto AEM.

1. Accedi a AEM Author
1. Accedi a __Sites > App WKND > us > it__
1. Seleziona la __Home page dell’app WKND__, e tocca __Proprietà__

   ![Home page dell’app WKND - Proprietà](./assets/aem-content/edit-home-properties.png)

1. Accedi a __SPA__ scheda
1. Compila il __Configurazione remota SPA__
   + __URL host SPA__: `http://localhost:3000`
      + URL della radice dell&#39;SPA remoto

   ![Home page dell’app WKND - Configurazione SPA remota](./assets/aem-content/remote-spa-configuration.png)

1. Tocca __Salva e chiudi__

Ricorda che abbiamo cambiato il tipo di questa pagina in quello di __Pagina SPA remoto__, che è ciò che ci consente di visualizzare __SPA__ scheda in __Proprietà pagina__.

Questa configurazione deve essere impostata solo sulla pagina AEM che corrisponde alla radice dell’SPA. Tutte le pagine AEM sotto questa pagina ereditano il valore.

## Complimenti

Hai preparato le configurazioni dell’AEM e le hai distribuite all’autore AEM locale. Ora sai come:

+ Rimuovi l’SPA generato da Archetipo progetto AEM commentando le dipendenze in `ui.frontend`
+ Aggiungere mappature Sling all’AEM che mappano le rotte dell’SPA alle risorse dell’AEM
+ Imposta i criteri di sicurezza per la condivisione delle risorse tra le origini dell’AEM che consentono all’SPA remoto di utilizzare contenuti provenienti dall’AEM
+ Distribuire il progetto AEM nel servizio di authoring dell’SDK AEM locale
+ Contrassegnare una pagina AEM come radice SPA remota utilizzando la proprietà di pagina URL host SPA

## Passaggi successivi

Con l&#39;AEM configurato, possiamo concentrarci su [avvio automatico dell’SPA remoto](./spa-bootstrap.md) con supporto per le aree modificabili tramite l’Editor SPA dell’AEM.
