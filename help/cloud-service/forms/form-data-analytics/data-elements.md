---
title: Generare rapporti sui campi dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per creare rapporti sui campi dati dei moduli
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# Creare elementi dati appropriati

Nella proprietà Tags sono stati aggiunti due nuovi elementi dati (ApplicantsStateOfResidence e validationError).

![modulo adattivo](assets/data_elements.png)

## StatoDiResidenzaRichiedente

Il **StatoDiResidenzaRichiedente** l’elemento dati è stato configurato selezionando **Core** nel menu a discesa dell’estensione e **Codice personalizzato** per il tipo di elemento dati, come illustrato nella schermata seguente
![Stato-richiedente-residenza](assets/applicantstateofresidence.png)

Il seguente codice personalizzato è stato utilizzato per acquisire il valore da **_stato_** campo modulo adattivo.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

Il **Errore di convalida** l’elemento dati è stato configurato selezionando **Core** nel menu a discesa dell’estensione e **Codice personalizzato** per il tipo di elemento dati, come illustrato nella schermata seguente

![validation-error](assets/validation-error.png)

Il seguente codice personalizzato è stato scritto per impostare il valore dell’elemento dati validationError.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```
