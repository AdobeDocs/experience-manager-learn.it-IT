---
title: Debug dell’SDK AEM tramite la console web OSGi
description: L’avvio rapido locale dell’SDK dell’AEM dispone di una console web OSGi che fornisce una serie di informazioni e introspezioni nel runtime dell’AEM locale che sono utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona nell’AEM.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 496
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 1%

---

# Debug dell’SDK AEM tramite la console web OSGi

L’avvio rapido locale dell’SDK dell’AEM dispone di una console web OSGi che fornisce una serie di informazioni e introspezioni nel runtime dell’AEM locale che sono utili per comprendere in che modo l’applicazione viene riconosciuta da e funziona nell’AEM.

AEM fornisce molte console OSGi, ciascuna delle quali fornisce informazioni chiave su diversi aspetti dell’AEM; tuttavia, i seguenti elementi sono in genere i più utili per il debug dell’applicazione.

## Bundle

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

La console Bundle è un catalogo dei bundle OSGi e dei relativi dettagli, distribuiti all’AEM, insieme alla possibilità specifica di avviarli e interromperli.

La console Bundles si trova in:

+ Strumenti > Operazioni > Console web > OSGi > Bundle
+ Oppure direttamente da: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Facendo clic su ciascun bundle, si forniscono dettagli utili per il debug dell’applicazione.

+ Convalida della presenza del bundle OSGi
+ Verifica dell’attivazione di un bundle OSGi
+ Determinare se un bundle OSGi contiene importazioni non soddisfatte che ne impediscono l’avvio

## Componenti

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

La console Componenti è un catalogo di tutti i componenti OSGi distribuiti a AEM e fornisce tutte le informazioni su di essi, dal ciclo di vita definito dei componenti OSGi a quali servizi OSGi possono fare riferimento

La console Componenti si trova in:

+ Strumenti > Operazioni > Console web > OSGi > Componenti
+ Oppure direttamente da: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Aspetti chiave utili per le attività di debug:

+ Convalida della presenza del bundle OSGi
+ Verifica dell’attivazione di un bundle OSGi
+ Determinare se un bundle OSGi contiene importazioni non soddisfatte che ne impediscono l’avvio
+ Ottenere il PID del componente per creare le relative configurazioni OSGi in Git
+ Identificazione dei valori delle proprietà OSGi associati alla configurazione OSGi attiva

## Modelli Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

La console Sling Models si trova in:

+ Strumenti > Operazioni > Console web > Stato > Modelli Sling
+ Oppure direttamente da: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Aspetti chiave utili per le attività di debug:

+ La convalida dei modelli Sling è registrata per il tipo di risorsa appropriato
+ La convalida dei modelli Sling può essere adattata dagli oggetti corretti (Resource o SlingHttpRequestServlet)
+ Convalida della corretta registrazione degli esportatori del modello Sling
