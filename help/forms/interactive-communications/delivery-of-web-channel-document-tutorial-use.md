---
title: Consegna del documento di comunicazione interattiva - Web Channel AEM Forms
seo-title: Consegna del documento di comunicazione interattiva - Web Channel AEM Forms
description: Consegna del documento del canale web tramite link in email
seo-description: Consegna del documento del canale web tramite link in email
feature: Comunicazione interattiva
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---


# Email Delivery of Web Channel Document

Una volta definito e verificato il documento di comunicazione interattiva del canale web, è necessario un meccanismo di consegna per consegnare il documento del canale web al destinatario.

In questo articolo, consideriamo l’e-mail come un meccanismo di consegna per il documento del canale web. Il destinatario riceverà un collegamento al documento del canale web via e-mail.Facendo clic sul collegamento, all&#39;utente verrà richiesto di eseguire l&#39;autenticazione e il documento del canale web verrà compilato con i dati specifici dell&#39;utente connesso.

Diamo un&#39;occhiata al seguente frammento di codice. Questo codice fa parte di GET.jsp che viene attivato quando l&#39;utente fa clic sul collegamento nell&#39;e-mail per visualizzare il documento del canale web. L&#39;utente connesso viene ottenuto utilizzando il jackrabbit UserManager. Una volta ottenuto l&#39;utente connesso, otteniamo il valore della proprietà accountNumber associata al profilo dell&#39;utente.

Associamo quindi il valore accountNumber a una chiave denominata numero di conto nella mappa. La chiave **accounts** è definita nel modale dei dati del modulo come attributo di richiesta. Il valore di questo attributo viene passato come parametro di input al metodo del servizio di lettura Form Data Modal.

Linea 7: Stiamo inviando la richiesta ricevuta a un altro servlet, in base al tipo di risorsa identificato dall’URL del documento di comunicazione interattiva. La risposta restituita da questo secondo servlet è inclusa nella risposta del primo servlet.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![includemethod](assets/includemethod.jpg)

Rappresentazione visiva del codice della riga 7

![requestparameter](assets/requestparameter.png)

Attributo di richiesta definito per il servizio di lettura del modale dei dati del modulo


[Pacchetto AEM di esempio](assets/webchanneldelivery.zip).
