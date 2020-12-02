---
title: Sviluppo con gli utenti dei servizi in  AEM Forms
seo-title: Sviluppo con gli utenti dei servizi in  AEM Forms
description: Questo articolo descrive il processo di creazione di un utente di servizi in  AEM Forms
seo-description: Questo articolo descrive il processo di creazione di un utente di servizi in  AEM Forms
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: adaptive-forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Sviluppo con gli utenti dei servizi in  AEM Forms

Questo articolo descrive il processo di creazione di un utente di servizi in  AEM Forms

Nelle versioni precedenti di Adobe Experience Manager (AEM), il risolutore delle risorse amministrative veniva utilizzato per l&#39;elaborazione back-end che richiedeva l&#39;accesso al repository. L&#39;utilizzo del risolutore risorse amministrative è obsoleto in AEM 6.3. Viene invece utilizzato un utente di sistema con autorizzazioni specifiche nella directory archivio.

In questo articolo viene descritta la creazione di un utente di sistema e la configurazione delle proprietà del mappatore utente.

1. Passa a [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Login come &#39; admin &#39;
1. Fare clic su &#39; Amministrazione utente &#39;
1. Fare clic su &quot; Crea utente di sistema &quot;
1. Impostate il tipo userid come &#39; data &#39; e fate clic sull&#39;icona verde per completare il processo di creazione dell&#39;utente del sistema
1. [Open configMgr](http://localhost:4502/system/console/configMgr)
1. Cercare &#39; Apache Sling Service User Mapper Service &#39; e fare clic per aprire le proprietà
1. Fate clic sull&#39;icona *+* (più) per aggiungere la mappatura dei servizi seguente

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Fare clic su &#39; Save &#39;

Nell’impostazione di configurazione sopra riportata, DevelopingWithServiceUser.core è il nome simbolico del bundle. getresources ceresolver è il nome del sottoservizio.data è l&#39;utente di sistema creato nel passaggio precedente.

È inoltre possibile ottenere il risolutore delle risorse per conto dell&#39;utente fd-service. Questo utente del servizio viene utilizzato per document services. Ad esempio, se desiderate certificare/applicare diritti di utilizzo ecc., utilizzeremo il risolutore delle risorse dell&#39;utente del servizio fd per eseguire le operazioni

1. [Scaricate e decomprimete il file zip associato a questo articolo.](assets/developingwithserviceuser.zip)
1. Passa a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Caricare e avviare il bundle OSGi
1. Verificare che il bundle sia in stato attivo
1. È stato creato un *utente di sistema* e distribuito anche il *pacchetto utente di servizio*.

   Per fornire l&#39;accesso a /content, assegnate all&#39;utente di sistema (&#39; dati &#39;) le autorizzazioni di lettura sul nodo del contenuto.

   1. Passa a [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Cercare i dati dell&#39;utente &#39; &#39;. Si tratta dello stesso utente di sistema creato nel passaggio precedente.
   1. Fare doppio clic sull&#39;utente, quindi fare clic sulla scheda Autorizzazioni
   1. Date a &#39; read&#39; l&#39;accesso alla cartella &#39;content&#39;.
   1. Per utilizzare l&#39;utente del servizio per accedere alla cartella /content, utilizzate il seguente codice

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Per accedere al file /content/dam/data.json nel pacchetto, utilizzate il seguente codice. Questo codice presuppone che siano state assegnate autorizzazioni di lettura all&#39;utente &quot;data&quot; sul nodo /content/dam/

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   Il codice completo dell&#39;attuazione è riportato di seguito

   ```java
   package com.mergeandfuse.getserviceuserresolver.impl;
   
   import java.util.HashMap;
   
   import org.apache.sling.api.resource.LoginException;
   import org.apache.sling.api.resource.ResourceResolver;
   import org.apache.sling.api.resource.ResourceResolverFactory;
   import org.osgi.service.component.annotations.Component;
   import org.osgi.service.component.annotations.Reference;
   import com.mergeandfuse.getserviceuserresolver.GetResolver;
   
   @Component(service = GetResolver.class)
   public class GetResolverImpl implements GetResolver {
    @Reference
    ResourceResolverFactory resolverFactory;
    @Override
    public ResourceResolver getServiceResolver() {
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

