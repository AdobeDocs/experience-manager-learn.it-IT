---
title: Popolare Adaptive Forms utilizzando i parametri di query.
description: Popolare Adaptive Forms con i dati provenienti dai parametri di query.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
source-git-commit: fad7630d2d91d03b98a3982f73a689ef48700319
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Precompilare l&#39;Adaptive Forms utilizzando i parametri di query

Uno dei nostri clienti aveva la necessità di compilare un modulo adattivo utilizzando i parametri di query. Ad esempio, nel seguente url i campi FirstName e LastName nel modulo adattivo sono impostati rispettivamente su John e Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Per eseguire questo caso d’uso è stato creato un nuovo modello di modulo adattivo e associato a un componente pagina. In questo componente di pagina abbiamo un jsp per ottenere i parametri di query e creare una struttura xml che può essere utilizzata per compilare il modulo adattivo.

I dettagli sulla creazione di un nuovo modello di modulo adattivo e del componente pagina sono [spiegato in questo video.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

Di seguito è riportato il codice utilizzato nella pagina jsp.

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>Se il modulo utilizza lo schema, la struttura del file xml sarà diversa e sarà necessario creare il file xml di conseguenza.


## Distribuire le risorse sul sistema

* [Scaricare e installare il modello di modulo adattivo utilizzando Gestione pacchetti](assets/populate-with-xml.zip)
* [Scarica e installa il modulo adattivo di esempio](assets/populate-af-with-query-paramters-form.zip)

* [Anteprima del modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Dovresti visualizzare il modulo adattivo con il valore John e Doe
