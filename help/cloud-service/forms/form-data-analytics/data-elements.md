---
title: Generare rapporti sui campi dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per creare rapporti sui campi dati dei moduli
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 42
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# Creare elementi dati

Nella proprietà Tags sono stati aggiunti due nuovi elementi dati (ApplicantsStateOfResidence e validationError).

![modulo adattivo](assets/data_elements.png)

## StatoDiResidenzaRichiedente

L&#39;elemento dati **ApplicantStateOfResidence** è stato configurato selezionando **Core** nel menu a discesa dell&#39;estensione e **Custom Code** per Data Element Type come mostrato nella schermata seguente
![residenza-stato-richiedente](assets/applicantstateofresidence.png)

Il seguente codice personalizzato è stato utilizzato per acquisire il valore dal campo del modulo adattivo **_state_**.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

L&#39;elemento dati **ValidationError** è stato configurato selezionando **Core** nel menu a discesa dell&#39;estensione e **Custom Code** per Data Element Type come mostrato nella schermata seguente

![errore di convalida](assets/validation-error.png)

Il seguente codice personalizzato è stato scritto per impostare il valore dell&#39;elemento dati `validationError`.

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