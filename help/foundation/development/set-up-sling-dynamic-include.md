---
title: Configurare Sling Dynamic Include per AEM
description: Una procedura video per l’installazione e l’utilizzo di Apache Sling Dynamic Include con AEM Dispatcher in esecuzione sul server web Apache HTTP.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Configura [!DNL Sling Dynamic Include]

Procedura video per l&#39;installazione e l&#39;utilizzo di [!DNL Apache Sling Dynamic Include] con [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=it) in esecuzione su [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> Assicurati che la versione più recente di AEM Dispatcher sia installata localmente.

1. Scarica e installa il [[!DNL Sling Dynamic Include] bundle](https://sling.apache.org/downloads.cgi).
1. Configura [!DNL Sling Dynamic Include] tramite [!DNL OSGi Configuration Factory] in **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   In alternativa, per aggiungere a una base di codice di AEM, crea il nodo **sling:OsgiConfig** appropriato in:

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

1. (Facoltativo) Ripetere l&#39;ultimo passaggio per consentire che i componenti in [contenuto bloccato (iniziale) di modelli modificabili](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/page-templates-editable.html) vengano serviti anche tramite [!DNL SDI]. Il motivo della configurazione aggiuntiva è che il contenuto bloccato dei modelli modificabili viene distribuito da `/conf` anziché da `/content`.

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

1. Aggiorna il file `httpd.conf` di [!DNL Apache HTTPD Web server] per abilitare il modulo [!DNL Include].

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Aggiorna il file [!DNL vhost] per rispettare le direttive di inclusione.

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

1. Aggiorna il file di configurazione dispatcher.any per supportare (1) `nocache` selettori e (2) abilitare il supporto TTL.

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
   > Se si lascia inattivo `*` finale nella regola glob `*.nocache.html*` precedente, si potrebbero verificare [problemi nelle richieste di risorse secondarie](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Riavvia sempre [!DNL Apache HTTP Web Server] dopo aver apportato modifiche ai file di configurazione o a `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Se utilizzi [!DNL Sling Dynamic Includes] per il server delle inclusioni lato perimetro (ESI), assicurati di memorizzare nella cache le [intestazioni di risposta rilevanti](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it#CachingHTTPResponseHeaders). Le intestazioni possibili includono:
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
