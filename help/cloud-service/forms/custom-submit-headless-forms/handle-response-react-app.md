---
title: Estrai risposta dall’invio personalizzato
description: Estrai risposta personalizzata all’invio corretto del modulo
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# Estrarre l’oggetto json dalla risposta

Quando invii un modulo adattivo headless a un gestore di invio personalizzato, i dati restituiti dal gestore di invio possono essere estratti e visualizzati nell’app di reazione. Il seguente frammento di codice mostra

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
