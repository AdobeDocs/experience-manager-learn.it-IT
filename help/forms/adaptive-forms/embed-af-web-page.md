---
title: Incorporazione di moduli Forms/HTML5 adattivi nella pagina web
description: Passaggi di configurazione necessari per incorporare moduli adattivi Forms o HTML5 in una pagina web non AEM.
feature: Moduli adattivi
feature-set: Adaptive Forms
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Amministrazione
role: Admin
level: Beginner
kt: 8390
source-git-commit: 2fc4f748fd3b8f820d1451d08c5fe01d11892029
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---


# Incorporazione di moduli adattivi o HTML5 in una pagina web

Il modulo adattivo incorporato è completamente funzionale e gli utenti possono compilare e inviare il modulo senza uscire dalla pagina. Consente all’utente di rimanere nel contesto di altri elementi della pagina web e di interagire contemporaneamente con il modulo.

Il video seguente spiega i passaggi necessari per incorporare un modulo adattivo o HTML5 nella pagina web.
Fai riferimento alla [documentazione](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) per i prerequisiti migliori, le best practice e così via.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

Puoi scaricare i file di esempio utilizzati nel video [da qui](assets/embedding-af-web-page.zip)

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














