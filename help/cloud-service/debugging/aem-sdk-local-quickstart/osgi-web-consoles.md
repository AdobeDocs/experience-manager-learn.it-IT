---
title: Debug AEM SDK tramite la console Web OSGi
description: L’avvio rapido locale dell’SDK AEM dispone di una console web OSGi che fornisce una varietà di informazioni e introspezioni nel runtime AEM locale utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona all’interno di AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 2%

---

# Debug AEM SDK tramite la console Web OSGi

L’avvio rapido locale dell’SDK AEM dispone di una console web OSGi che fornisce una varietà di informazioni e introspezioni nel runtime AEM locale utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona all’interno di AEM.

AEM fornisce molte console OSGi, ognuna delle quali fornisce informazioni chiave su diversi aspetti di AEM, tuttavia i seguenti sono tipicamente i più utili per il debug dell’applicazione.

## Bundle

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

La console Bundles è un catalogo dei bundle OSGi e dei loro dettagli, distribuiti per AEM, insieme alla possibilità ad hoc di avviarli e fermarli.

La console Bundle si trova in:

+ Strumenti > Operazioni > Console web > OSGi > Bundle
+ Oppure direttamente da: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Facendo clic su ogni bundle, vengono forniti dettagli utili per il debug dell&#39;applicazione.

+ Convalida del bundle OSGi presente
+ Convalida se un bundle OSGi è attivo
+ Determinare se un bundle OSGi ha effettuato importazioni non soddisfatte impedendo l&#39;avvio di tale bundle

## Componenti

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

La console Componenti è un catalogo di tutti i componenti OSGi implementati in AEM e fornisce tutte le informazioni su di essi, dal loro ciclo di vita del componente OSGi definito, ai servizi OSGi a cui possono fare riferimento.

La console Componenti si trova in:

+ Strumenti > Operazioni > Console web > OSGi > Componenti
+ Oppure direttamente da: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Aspetti chiave delle attività di debug:

+ Convalida del bundle OSGi presente
+ Convalida se un bundle OSGi è attivo
+ Determinare se un bundle OSGi ha effettuato importazioni non soddisfatte impedendo l&#39;avvio di tale bundle
+ Ottenere il PID del componente, per creare configurazioni OSGi per loro in Git
+ Identificazione dei valori delle proprietà OSGi associati alla configurazione OSGi attiva

## Modelli Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

La console Sling Models si trova in:

+ Strumenti > Operazioni > Console web > Stato > Modelli Sling
+ Oppure direttamente da: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Aspetti chiave delle attività di debug:

+ Convalida dei modelli Sling registrati nel tipo di risorsa appropriato
+ La convalida dei modelli Sling è adattabile dagli oggetti corretti (Resource o SlingHttpRequestServlet)
+ La convalida degli esportatori di modelli Sling è registrata correttamente
