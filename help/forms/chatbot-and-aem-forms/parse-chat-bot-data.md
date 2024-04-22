---
title: Utilizzo di AEM Forms con Chatbot
description: Analizzare i dati di ChatBot
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# Analizzare i dati di ChatBot

A [Webhook ChatBot](https://www.chatbot.com/help/webhooks/what-are-webhooks/) è stato utilizzato per inviare i dati ChatBot a un servlet AEM.
I dati acquisiti in ChatBot sono in formato JSON e l’utente ha inserito i dati nell’oggetto attributi come mostrato di seguito
![chatbot-data](assets/chat-bot-data.png)

Per unire i dati con il modello XDP, è necessario creare il seguente XML. Osserva l’elemento principale del file xml, che deve corrispondere all’elemento principale del modello XDP per consentire la corretta unione dei dati.


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![xdp-template](assets/xdp-template.png)

## Passaggi successivi

[Unire i dati con il modello XDP](./merge-data-with-template.md)



