---
title: Imposta Sling Dynamic Include per AEM
description: Una guida dettagliata all’installazione e all’utilizzo di Apache Sling Dynamic Include con AEM Dispatcher in esecuzione su Apache HTTP Web Server.
version: 6.4, 6.5
sub-product: foundation, sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 5%

---

# Configurazione [!DNL Sling Dynamic Include]

Una guida video dettagliata sull’installazione e l’utilizzo di [!DNL Apache Sling Dynamic Include] con [Dispatcher AEM](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it) in esecuzione [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Assicurati che AEM Dispatcher sia installata localmente la versione più recente.

1. Scarica e installa la [[!DNL Sling Dynamic Include] fascio](https://sling.apache.org/downloads.cgi).
1. Configura [!DNL Sling Dynamic Include] tramite [!DNL OSGi Configuration Factory] a **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dinamicinclude.Configuration**.

   Oppure, per aggiungere a una base di codice AEM, crea la **sling:OsgiConfig** nodo in:

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

1. (Facoltativo) Ripeti l’ultimo passaggio per consentire l’attivazione dei componenti [contenuto bloccato (iniziale) dei modelli modificabili](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/page-templates-editable.html) da servire tramite [!DNL SDI] anche. La ragione della configurazione aggiuntiva è che il contenuto bloccato dei modelli modificabili viene servito da `/conf` anziché `/content`.

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

1. Aggiorna [!DNL Apache HTTPD Web server]s `httpd.conf` per abilitare [!DNL Include] modulo .

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Aggiorna [!DNL vhost] file da rispettare include direttive.

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

1. Aggiorna il file di configurazione dispatcher.any per supportare (1) `nocache` e (2) abilitare il supporto TTL.

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
   > Lasciare il finale `*` fuori nel glob `*.nocache.html*` regola precedente, può [problemi nelle richieste di risorse secondarie](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Riavvia sempre [!DNL Apache HTTP Web Server] dopo aver apportato modifiche ai relativi file di configurazione o `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Se utilizzi [!DNL Sling Dynamic Includes] per il server edge-side include (ESI), assicurati di memorizzare nella cache le informazioni rilevanti [intestazioni di risposta nella cache del dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Le intestazioni possibili includono:
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

* [Scarica il bundle Sling Dynamic Include](https://sling.apache.org/downloads.cgi)
* [Documentazione di Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)
