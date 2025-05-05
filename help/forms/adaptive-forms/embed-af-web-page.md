---
title: Incorporazione di moduli adattivi Forms/HTML5 nella pagina web
description: Passaggi di configurazione necessari per incorporare moduli Adaptive Forms o HTML5 in una pagina web non AEM.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 398
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Incorporazione di moduli adattivi o moduli HTML5 nella pagina web

Il modulo adattivo incorporato è completamente funzionante e gli utenti possono compilare e inviare il modulo senza uscire dalla pagina. Consente all’utente di rimanere nel contesto di altri elementi della pagina web e interagire contemporaneamente con il modulo.

Il video seguente illustra i passaggi necessari per incorporare un modulo adattivo o HTML5 in una pagina web.
Fai riferimento alla [documentazione](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=it) per i prerequisiti e le best practice, ecc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

Puoi scaricare i file di esempio usati nel video [da qui](assets/embedding-af-web-page.zip)

Di seguito è riportato il codice utilizzato per recuperare il modulo adattivo e incorporarlo nel contenitore identificato dal nome di classe **right**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
