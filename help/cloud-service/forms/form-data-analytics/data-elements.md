---
title: Generare rapporti sui campi dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per creare rapporti sui campi dati dei moduli
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 53
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# Creare elementi dati

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
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

Il **Errore di convalida** l’elemento dati è stato configurato selezionando **Core** nel menu a discesa dell’estensione e **Codice personalizzato** per il tipo di elemento dati, come illustrato nella schermata seguente

![validation-error](assets/validation-error.png)

Il seguente codice personalizzato è stato scritto per impostare `validationError` valore dell’elemento dati.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## Passaggi successivi

[Creare le regole](./rules.md)