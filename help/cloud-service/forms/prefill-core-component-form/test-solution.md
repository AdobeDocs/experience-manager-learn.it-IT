---
title: Modulo adattivo basato su Componente core precompilato
description: Scopri come precompilare un modulo adattivo con i dati
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
source-git-commit: d9612adbc2ff3e601c2efe5a779c03ad24769276
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Testare la soluzione

Dopo la distribuzione del codice, crea un modulo adattivo basato sui componenti core. Associa il modulo adattivo al servizio di precompilazione come mostrato nella schermata seguente.
![preriempimento-servizio](assets/pre-fill-service.png)

Ogni volta che viene eseguito il rendering del modulo, viene eseguito il servizio di precompilazione associato e il modulo viene compilato con i dati restituiti dal servizio di precompilazione.

Ad esempio, per precompilare il modulo con i dati associati al GUID **d815a2b3-5f4c-4422-8197-d0b73479bf0e**, viene utilizzato il seguente URL.
Il codice nel servizio di precompilazione estrae il valore del parametro guid e recupera i dati associati al guid dall&#39;origine dati.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
