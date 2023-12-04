---
title: Visualizza l’ID di invio all’invio del modulo
description: Visualizzare la risposta di un modello dati modulo nella pagina di ringraziamento
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13900
last-substantial-update: 2023-09-09T00:00:00Z
exl-id: 18648914-91cc-470d-8f27-30b750eb2f32
duration: 98
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Personalizza pagina di ringraziamento

Quando invii un modulo adattivo a un endpoint REST, vuoi mostrare un messaggio di conferma per informare l’utente che l’invio del modulo è andato a buon fine. La risposta del POST contiene dettagli sull’invio, ad esempio l’ID invio, e un messaggio di conferma ben progettato include l’ID invio, contribuendo a migliorare l’esperienza utente. Questa risposta può essere visualizzata nella pagina di ringraziamento configurata con il modulo adattivo.

La schermata seguente mostra un modulo inviato utilizzando l’azione Invia modello dati modulo con una pagina di ringraziamento configurata

![pagina di ringraziamento](./assets/thank-you-page-fdm-submit.png)

Il POST di un modello dati modulo restituirà sempre un oggetto JSON nella risposta. Questo JSON è disponibile nell’URL della pagina di ringraziamento come parametro di query denominato _fdmSubmitResult_. Puoi analizzare questo parametro di query e visualizzare gli elementi JSON nella pagina di ringraziamento.
Il codice di esempio seguente analizza la risposta JSON per estrarre il valore del campo number. Il codice xml appropriato viene quindi costruito e trasmesso in slingRequest per compilare il modulo. Questo codice viene in genere scritto nel file jsp del componente page associato al modello di modulo adattivo.

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

Si consiglia di basare la pagina di ringraziamento su un nuovo modello di modulo adattivo che consente di scrivere il codice personalizzato per estrarre la risposta dai parametri di query.

## Testare la soluzione

Crea un modulo adattivo e configura l’invio del modulo utilizzando l’azione di invio Modello dati modulo.
[Distribuire il modello di modulo adattivo di esempio](assets/thank-you-page-template.zip)
Crea un modulo di ringraziamento basato su questo modello Associa questa pagina di ringraziamento al modulo principale Modifica il codice JSP in [createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp) per generare l’xml necessario per precompilare il modulo adattivo.
Visualizza l’anteprima e invia il modulo adattivo.
La pagina di ringraziamento deve essere visualizzata e precompilata con i dati specificati nel file XML
