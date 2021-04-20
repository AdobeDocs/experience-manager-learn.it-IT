---
title: Sviluppo con gli utenti dei servizi in AEM Forms
seo-title: Sviluppo con gli utenti dei servizi in AEM Forms
description: Questo articolo illustra il processo di creazione di un utente di servizio in AEM Forms
seo-description: Questo articolo illustra il processo di creazione di un utente di servizio in AEM Forms
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: Adaptive Forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# Sviluppo con gli utenti dei servizi in AEM Forms

Questo articolo illustra il processo di creazione di un utente di servizio in AEM Forms

Nelle versioni precedenti di Adobe Experience Manager (AEM), il risolutore di risorse amministrative veniva utilizzato per l’elaborazione back-end che richiedeva l’accesso all’archivio. L’utilizzo del risolutore risorse amministrative è obsoleto in AEM 6.3. Viene invece utilizzato un utente di sistema con autorizzazioni specifiche nell’archivio.

Questo articolo illustra la creazione di un utente di sistema e la configurazione delle proprietà di mappatura utente.

1. Passa a [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Accedi come amministratore &#39;
1. Fai clic su &#39; Amministrazione utente &#39;
1. Fai clic su &quot;Crea utente di sistema&quot;
1. Imposta il tipo userid come &#39; data &#39; e fai clic sull&#39;icona verde per completare il processo di creazione dell&#39;utente di sistema
1. [Apri configMgr](http://localhost:4502/system/console/configMgr)
1. Cerca &#39; Apache Sling Service User Mapper Service &#39; e fai clic per aprire le proprietà
1. Fai clic sull&#39;icona *+* (più) per aggiungere la seguente Mappatura del servizio

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Fai clic su &#39; Salva &#39;

Nell&#39;impostazione di configurazione di cui sopra DevelopingWithServiceUser.core è il nome simbolico del bundle. getresourceresolver è il nome del sottoservizio.data è l&#39;utente di sistema creato nel passaggio precedente.

Possiamo anche ottenere il risolutore risorse per conto dell&#39;utente di servizio fd. Questo utente di servizio viene utilizzato per document services. Ad esempio, se desideri certificare/applicare diritti di utilizzo, ecc., utilizzeremo il risolutore di risorse dell&#39;utente di servizio fd per eseguire le operazioni

1. [Scarica e decomprimi il file zip associato a questo articolo.](assets/developingwithserviceuser.zip)
1. Passa a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Carica e avvia il bundle OSGi
1. Assicurati che il bundle sia in stato attivo
1. Ora hai creato correttamente un *utente di sistema* e hai implementato anche il *Service User bundle*.

   Per fornire l&#39;accesso a /content, assegna all&#39;utente di sistema (&#39; dati &#39;) le autorizzazioni di lettura sul nodo del contenuto.

   1. Passa a [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Cerca i dati dell&#39;utente &#39;. Si tratta dello stesso utente di sistema creato nel passaggio precedente.
   1. Fai doppio clic sull&#39;utente e quindi fai clic sulla scheda &quot;Autorizzazioni&quot;
   1. Assegna a &#39; read &#39; l&#39;accesso alla cartella &#39;content&#39;.
   1. Per utilizzare l&#39;utente del servizio per ottenere l&#39;accesso alla cartella /content, utilizza il seguente codice

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Se desideri accedere al file /content/dam/data.json nel tuo bundle, userai il seguente codice. Questo codice presuppone che tu abbia dato le autorizzazioni di lettura all&#39;utente &quot;data&quot; sul nodo /content/dam/

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

