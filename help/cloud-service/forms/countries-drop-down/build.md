---
title: Creare, distribuire e testare il componente Paesi
description: Genera, distribuisci e testa
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Creare, distribuire e testare il componente Paesi

Per generare tutti i moduli e distribuire il pacchetto `all` in un&#39;istanza locale di AEM, eseguire il comando seguente nella directory principale del progetto:

```mvn clean install -PautoInstallSinglePackage```

## Testare il componente

Per integrare il componente Paesi nell’istanza AEM Forms Cloud Ready e configurarlo, effettua le seguenti operazioni:

* Estrai il contenuto del file zip [countries](assets/countries.zip). Ogni file deve contenere i dati per un continente specifico.
* Carica i file JSON in content/dam/corecomponent.This è la posizione in cui il codice cerca i file JSON.Se desideri memorizzare i file JSON in una posizione diversa, dovrai aggiornare il codice Java nella classe CountriesDropDownImpl. In particolare, aggiorna il percorso nel metodo init() in cui vengono caricati i file JSON. Ad esempio, se desideri memorizzare i file JSON in content/dam/mydata/, aggiorna il percorso in questo modo

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Accedi all’istanza di AEM Forms Cloud Ready
* Creare un modulo adattivo e rilasciare il componente Paesi sul modulo
* Configura il componente Paesi utilizzando l’editor di finestre di dialogo e imposta le varie proprietà, incluso il continente.
  ![contenuto](assets/select-continent.png)
* Visualizza l’anteprima del modulo e assicurati che il menu a discesa Paesi funzioni come previsto
