---
title: Configurare AEM per SPA Editor e SPA remoto
description: È necessario un progetto AEM per configurare il supporto della configurazione e dei requisiti di contenuto per consentire AEM Editor SPA creare un SPA remoto.
topic: Senza testa, SPA, Sviluppo
feature: Editor SPA, componenti core, API, sviluppo
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1220'
ht-degree: 1%

---


# Configurare AEM per l’editor SPA

Mentre la base di codice SPA è gestita al di fuori di AEM, è necessario un progetto AEM per impostare il supporto della configurazione e dei requisiti di contenuto. Questo capitolo illustra la creazione di un progetto AEM che contiene le configurazioni necessarie:

+ AEM proxy dei componenti core WCM
+ AEM proxy SPA pagina remota
+ AEM modelli di pagina SPA remoto
+ SPA remote di base AEM pagine
+ Sottoprogetto per definire SPA per AEM mappature URL
+ Cartelle di configurazione OSGi

## Creare un progetto AEM

Crea un progetto AEM in cui vengono gestite le configurazioni e il contenuto della linea di base.

_Utilizza sempre la versione più recente di  [AEM Archetype](https://github.com/adobe/aem-project-archetype)._


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_L&#39;ultimo comando semplicemente rinomina la cartella del progetto AEM in modo che sia chiaro che si tratta del progetto AEM e non deve essere confuso con Remote SPA__

Mentre è specificato `frontendModule="react"`, il progetto `ui.frontend` non viene utilizzato per il caso d’uso SPA remoto. Il SPA viene sviluppato e gestito esternamente per AEM e utilizza solo AEM come API di contenuto. Il flag `frontendModule="react"` è necessario per il progetto, include le dipendenze `spa-project` AEM Java™ e imposta i modelli di pagina SPA remoti.

AEM Project Archetype genera i seguenti elementi utilizzati per configurare AEM per l’integrazione con l’SPA.

+ __AEM__ proxy dei componenti core WCM  `ui.content/src/.../apps/wknd-app/components`
+ __AEM SPA__ proxy pagina remota  `ui.content/src/.../apps/wknd-app/components/remotepage`
+ __AEM__ Modelli di pagina  `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Sottoprogetto per definire la__ mappatura del contenuto  `ui.content/src/...`
+ __SPA remoto linea di base AEM__ pagine  `ui.content/src/.../content/wknd-app`
+ __Cartella__ di configurazione OSGi  `ui.config/src/.../apps/wknd-app/osgiconfig`

Con il progetto di base AEM generato, alcune regolazioni garantiscono la compatibilità SPA Editor con il SPA remoto.

## Rimuovi progetto ui.frontend

Poiché il SPA è un SPA remoto, si presuppone che sia sviluppato e gestito al di fuori del progetto AEM. Per evitare conflitti, rimuovi il progetto `ui.frontend` dalla distribuzione. Se il progetto `ui.frontend` non viene rimosso, due SPA, il SPA predefinito fornito nel progetto `ui.frontend` e nel SPA remoto, verranno caricati contemporaneamente nell’Editor di SPA AEM.

1. Apri il progetto AEM (`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`) nell’IDE
1. Apri la radice `pom.xml`
1. Aggiungi un commento a `<module>ui.frontend</module` dall&#39;elenco `<modules>`

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

   Il file `pom.xml` deve essere simile al seguente:

   ![Rimuovi modulo ui.frontend da reactor pom](./assets/aem-project/uifrontend-reactor-pom.png)

1. Apri `ui.apps/pom.xml`
1. Invia un commento a `<dependency>` il `<artifactId>wknd-app.ui.frontend</artifactId>`

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

   Il file `ui.apps/pom.xml` deve essere simile al seguente:

   ![Rimuovere la dipendenza ui.frontend da ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

Se il progetto AEM è stato generato prima di queste modifiche, elimina manualmente la libreria client generata `ui.frontend` dal progetto `ui.apps` in `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## Mappatura del contenuto AEM

Affinché AEM caricare il SPA remoto nell’Editor SPA, è necessario stabilire le mappature tra le route SPA e le pagine AEM utilizzate per aprire e creare contenuti.

L&#39;importanza di questa configurazione viene esplorata in seguito.

La mappatura può essere effettuata con [Sling Mapping](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) definito in `/etc/map`.

1. Nell’IDE, apri il sottoprogetto `ui.content`
1. Accedi a  `src/main/content/jcr_root/etc`
1. Crea una cartella . `map`
1. In `map`, crea una cartella `http`
1. In `http`, crea un file `.content.xml` con il contenuto:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. In `http` , crea una cartella `localhost_any`
1. In `localhost_any`, crea un file `.content.xml` con il contenuto:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. In `localhost_any` , crea una cartella `wknd-app-routes-adventure`
1. In `wknd-app-routes-adventure`, crea un file `.content.xml` con il contenuto:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages will be created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Aggiungi i nodi di mappatura a `ui.content/src/main/content/META-INF/vault/filter.xml` per includerli nel pacchetto AEM.

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

La struttura delle cartelle e i file `.context.xml` devono avere un aspetto simile al seguente:

![Mappatura Sling](./assets/aem-project/sling-mapping.png)

Il file `filter.xml` deve essere simile al seguente:

![Mappatura Sling](./assets/aem-project/sling-mapping-filter.png)

Ora, quando il progetto AEM viene distribuito, queste configurazioni vengono incluse automaticamente.

Gli effetti di mappatura Sling AEM in esecuzione su `http` e `localhost`, quindi supportano solo lo sviluppo locale. Quando distribuisci a AEM come Cloud Service, devono essere aggiunte mappature Sling simili a quelle di destinazione `https` e alla AEM appropriata come domini di Cloud Service. Per ulteriori informazioni, consulta la [documentazione relativa alla mappatura Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Criteri di sicurezza per la condivisione delle risorse tra origini

Quindi, configura AEM per proteggere il contenuto in modo che solo questo SPA possa accedere al contenuto AEM. C Configurare la condivisione risorse tra le origini [in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. Nell’IDE, apri il sottoprogetto `ui.config` Maven
1. Naviga `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Crea un file denominato `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Aggiungi quanto segue al file :

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
           "Authorization"
       ]
   }
   ```

Il file `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` deve essere simile al seguente:

![Configurazione di SPA Editor CORS](./assets/aem-project/cors-configuration.png)

Gli elementi di configurazione chiave sono:

+ `alloworigin` specifica gli host autorizzati a recuperare il contenuto da AEM.
   + `localhost:3000` viene aggiunto per supportare l’SPA in esecuzione localmente
   + `https://external-hosted-app` funge da segnaposto da sostituire con il dominio su cui è ospitato il SPA remoto.
+ `allowedpaths` specifica quali percorsi in AEM sono coperti da questa configurazione CORS. L’impostazione predefinita consente l’accesso a tutto il contenuto di AEM, tuttavia questo può essere limitato solo ai percorsi specifici a cui l’SPA può accedere, ad esempio: `/content/wknd-app`.

## Imposta AEM pagina come modello di pagina SPA remoto

AEM Project Archetype genera un progetto preparato per l’integrazione AEM con un SPA remoto, ma richiede una regolazione piccola ma importante per la struttura AEM pagina generata automaticamente. Il tipo della pagina AEM generata automaticamente deve essere stato modificato in __Pagina SPA remota__, anziché in una __SPA pagina__.

1. Nell’IDE, apri il sottoprogetto `ui.content`
1. Apri in `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Aggiorna il file `.content.xml` con:

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

Le modifiche chiave sono aggiornamenti al nodo `jcr:content` :

+ `cq:template` a `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` a `wknd-app/components/remotepage`

Il file `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` deve essere simile al seguente:

![Aggiornamenti .content.xml della home page](./assets/aem-project/home-content-xml.png)

Queste modifiche consentono a questa pagina, che agisce come radice SPA in AEM, di caricare il SPA remoto in SPA Editor.

>[!NOTE]
>
>Se in precedenza il progetto doveva AEM, assicurati di eliminare la pagina AEM come __Sites > WKND App > us > en > WKND App Home Page__, in quanto il progetto `ui.content` è impostato sui nodi __merge__, anziché sui nodi __update__.

È anche possibile rimuovere e ricreare la pagina come Pagina SPA remota AEM stessa, tuttavia, poiché questa pagina viene creata automaticamente nel progetto `ui.content`, è consigliabile aggiornarla nella base di codice.

## Distribuire il progetto AEM per AEM SDK

1. Assicurati che il servizio AEM Author sia in esecuzione sulla porta 4502
1. Dalla riga di comando, accedi alla directory principale del progetto Maven AEM
1. Utilizza Maven per distribuire il progetto al servizio locale AEM SDK Author

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## Configurare la pagina AEM principale

Con il progetto AEM implementato, c&#39;è un ultimo passaggio per preparare SPA editor per caricare il nostro SPA remoto. In AEM, contrassegna la pagina AEM che corrisponde alla radice SPA,`/content/wknd-app/us/en/home`, generata dal AEM Project Archetype.

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND > us > en__
1. Seleziona la __home page dell&#39;app WKND__ e tocca __Proprietà__

   ![Home page app WKND - Proprietà](./assets/aem-content/edit-home-properties.png)

1. Passa alla scheda __SPA__
1. Compila la __Configurazione SPA remota__
   + __URL__ host SPA:  `http://localhost:3000`
      + URL della directory principale del SPA remoto

   ![Home page app WKND - Configurazione SPA remota](./assets/aem-content/remote-spa-configuration.png)

1. Tocca __Salva e chiudi__

Ricorda che il tipo di questa pagina è stato modificato in quello di una __Pagina SPA remota__, che è ciò che ci consente di visualizzare la scheda __SPA__ nella relativa scheda __Proprietà pagina__.

Questa configurazione deve essere impostata solo sulla pagina AEM che corrisponde alla radice del SPA. Tutte le pagine AEM sotto questa pagina ereditano il valore .

## Congratulazioni

Ora hai preparato AEM configurazioni e le hai distribuite al tuo autore AEM locale! Ora sai come:

+ Rimuovi il SPA generato da AEM Archetipo di progetto, commentando le dipendenze in `ui.frontend`
+ Aggiungi mappature Sling a AEM che mappano i percorsi SPA alle risorse in AEM
+ Configurare AEM criteri di sicurezza per la condivisione delle risorse tra origini che consentono al SPA remoto di utilizzare contenuti da AEM
+ Implementare il progetto AEM nel servizio locale AEM SDK Author
+ Contrassegnare una pagina AEM come radice SPA remota utilizzando la proprietà della pagina URL host SPA

## Passaggi successivi

Con AEM configurato, possiamo concentrarci sull&#39; [avvio del SPA remoto](./spa-bootstrap.md) con il supporto per le aree modificabili tramite AEM Editor!