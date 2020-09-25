---
title: Debug AEM SDK tramite la console Web OSGi
description: L’avvio rapido locale dell’SDK AEM dispone di una console Web OSGi che fornisce una serie di informazioni e introspettive nel runtime AEM locale utili per comprendere in che modo l’applicazione viene riconosciuta e funziona all’interno di AEM.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 1%

---


# Debug AEM SDK tramite la console Web OSGi

L’avvio rapido locale dell’SDK AEM dispone di una console Web OSGi che fornisce una serie di informazioni e introspettive nel runtime AEM locale utili per comprendere in che modo l’applicazione viene riconosciuta e funziona all’interno di AEM.

AEM diverse console OSGi, ciascuna delle quali fornisce informazioni chiave su diversi aspetti dei AEM, tuttavia i seguenti sono in genere i più utili per il debug dell’applicazione.

## Bundle

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

La console Bundles è un catalogo dei bundle OSGi e dei relativi dettagli, distribuiti per AEM, insieme alla possibilità ad hoc di avviarli e arrestarli.

La console Bundles si trova in:

+ Strumenti > Operazioni > Console Web > OSGi > Bundle
+ Or directly at: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Facendo clic su ciascun bundle, vengono forniti i dettagli utili per il debug dell&#39;applicazione.

+ Convalida del bundle OSGi presente
+ Convalida se un bundle OSGi è attivo
+ Determinare se un bundle OSGi ha importazioni non soddisfatte e non è possibile avviarlo

## Componenti

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

La console Componenti è un catalogo di tutti i componenti OSGi distribuiti per AEM e fornisce tutte le informazioni su di essi, dal loro ciclo di vita dei componenti OSGi definito, ai servizi OSGi a cui possono fare riferimento

La console Componenti si trova in:

+ Strumenti > Operazioni > Console Web > OSGi > Componenti
+ Or directly at: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Aspetti chiave che aiutano con le attività di debug:

+ Convalida del bundle OSGi presente
+ Convalida se un bundle OSGi è attivo
+ Determinare se un bundle OSGi ha importazioni non soddisfatte e non è possibile avviarlo
+ Ottenimento del PID del componente, per creare configurazioni OSGi per essi in Git
+ Identificazione dei valori delle proprietà OSGi associati alla configurazione OSGi attiva

## Modelli Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

La console Sling Models si trova in:

+ Strumenti > Operazioni > Console Web > Stato > Modelli Sling
+ Or directly at: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Aspetti chiave che aiutano con le attività di debug:

+ Convalida dei modelli Sling registrati nel tipo di risorsa appropriato
+ La convalida dei modelli Sling è adattabile dagli oggetti corretti (Resource o SlingHttpRequestServlet)
+ Convalida degli esportatori di modelli Sling corretta
