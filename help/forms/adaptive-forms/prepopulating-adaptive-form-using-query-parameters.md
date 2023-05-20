---
title: Popola il Forms adattivo utilizzando i parametri di query.
description: Popola Forms adattivo con i dati dei parametri di query.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# PrePopolare Forms adattivo utilizzando i parametri di query

Uno dei nostri clienti aveva il requisito di compilare il modulo adattivo utilizzando i parametri di query. Ad esempio, nell’URL seguente i campi FirstName e LastName nel modulo adattivo sono impostati rispettivamente su John e Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Per eseguire questo caso d’uso è stato creato un nuovo modello di modulo adattivo associato a un componente pagina. In questo componente pagina è disponibile una JSP per l’acquisizione dei parametri di query e la creazione di una struttura xml da utilizzare per compilare il modulo adattivo.

I dettagli sulla creazione di un nuovo modello di modulo adattivo e di un nuovo componente pagina sono [spiegato in questo video.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

Di seguito è riportato il codice utilizzato nella pagina jsp

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
>Se il modulo utilizza uno schema, la struttura del file xml sarà diversa e sarà necessario generarlo di conseguenza.


## Distribuire le risorse sul sistema

* [Scaricare e installare il modello di modulo adattivo utilizzando Gestione pacchetti](assets/populate-with-xml.zip)
* [Scarica e installa il modulo adattivo di esempio](assets/populate-af-with-query-paramters-form.zip)

* [Anteprima del modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Dovresti vedere il modulo adattivo compilato con i valori John e Doe
