---
title: Imposta Sling Dynamic Include per AEM
description: Video introduttivo sull’installazione e l’utilizzo di Apache Sling Dynamic Include con AEM Dispatcher in esecuzione sul server Web Apache HTTP.
version: 6.3, 6.4, 6.5
sub-product: fondazione, siti
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 3%

---


# Imposta [!DNL Sling Dynamic Include]

Video sull&#39;installazione e l&#39;utilizzo di [!DNL Apache Sling Dynamic Include] con [AEM Dispatcher](https://docs.adobe.com/content/help/it-IT/experience-manager-dispatcher/using/dispatcher.html) in esecuzione su [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Verificare che la versione più recente di AEM Dispatcher sia installata localmente.

1. Scaricate e installate il [[!DNL Sling Dynamic Include] bundle](https://sling.apache.org/downloads.cgi).
1. Configurare [!DNL Sling Dynamic Include] tramite [!DNL OSGi Configuration Factory] all&#39;indirizzo **http://&lt;host>:&lt;porta>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Oppure, per aggiungere a una base di codice AEM, create il nodo **sling:OsgiConfig** appropriato all&#39;indirizzo:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (Facoltativo) Ripetete l&#39;ultimo passaggio per consentire la distribuzione tramite [!DNL SDI] anche dei componenti con contenuto [bloccato (iniziale) di modelli modificabili](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html). La ragione per la configurazione aggiuntiva è che il contenuto bloccato dei modelli modificabili viene distribuito da `/conf` invece di `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. Aggiornare il file [!DNL Apache HTTPD Web server] di `httpd.conf` per abilitare il modulo [!DNL Include].

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Aggiornate il file [!DNL vhost] per rispettare le direttive di inclusione.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. Aggiornate il file di configurazione dispatcher.any per supportare (1) `nocache` selettori e (2) abilitare il supporto TTL.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > Se si esce dalla regola `*` finale nella regola di tipo `*.nocache.html*`, è possibile che si verifichino problemi di [richieste di risorse secondarie](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Riavviare sempre [!DNL Apache HTTP Web Server] dopo aver apportato modifiche ai relativi file di configurazione o al `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Se si utilizza [!DNL Sling Dynamic Includes] per la trasmissione di include sul lato periferico (ESI), assicurarsi di memorizzare nella cache le intestazioni di risposta [rilevanti nella cache del dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Le intestazioni possibili sono le seguenti:
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;Scade&quot;
>* &quot;Ultima modifica&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Ultima modifica&quot;

>



## Materiali di supporto

* [Download del bundle Sling Dynamic Include](https://sling.apache.org/downloads.cgi)
* [Documentazione di Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)
