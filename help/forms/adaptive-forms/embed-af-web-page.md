---
title: Incorporazione di moduli Forms/HTML5 adattivi nella pagina web
description: Passaggi di configurazione necessari per incorporare moduli adattivi Forms o HTML5 in una pagina web non AEM.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# Incorporazione di moduli adattivi o HTML5 in una pagina web

Il modulo adattivo incorporato è completamente funzionale e gli utenti possono compilare e inviare il modulo senza uscire dalla pagina. Consente all’utente di rimanere nel contesto di altri elementi della pagina web e di interagire contemporaneamente con il modulo.

Il video seguente illustra i passaggi necessari per incorporare un modulo adattivo o HTML5 nella pagina web.
Fai riferimento alla [documentazione](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) per i prerequisiti migliori, le best practice e così via
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

È possibile scaricare i file di esempio utilizzati nel video [da qui](assets/embedding-af-web-page.zip)

Di seguito è riportato il codice utilizzato per recuperare il modulo adattivo e incorporare il modulo nel contenitore identificato dal nome della classe **right**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
