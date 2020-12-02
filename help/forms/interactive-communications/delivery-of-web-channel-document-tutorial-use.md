---
title: Consegna del documento di comunicazione interattiva - Canale Web  AEM Forms
seo-title: Consegna del documento di comunicazione interattiva - Canale Web  AEM Forms
description: Distribuzione del documento del canale Web tramite collegamento nel messaggio e-mail
seo-description: Distribuzione del documento del canale Web tramite collegamento nel messaggio e-mail
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# Distribuzione tramite e-mail del documento del canale Web

Dopo aver definito e verificato il documento di comunicazione interattiva per il canale Web, è necessario un meccanismo di distribuzione per distribuire il documento per il canale Web al destinatario.

In questo articolo, diamo un&#39;occhiata alle e-mail come un meccanismo di consegna per i documenti di canale Web. Il destinatario riceverà un collegamento al documento del canale Web via e-mail.Facendo clic sul collegamento, all&#39;utente verrà chiesto di effettuare l&#39;autenticazione e il documento del canale Web verrà compilato con i dati specifici dell&#39;utente che ha effettuato l&#39;accesso.

Diamo un&#39;occhiata al frammento di codice seguente. Questo codice fa parte di GET.jsp che viene attivato quando l&#39;utente fa clic sul collegamento nell&#39;e-mail per visualizzare il documento del canale Web. L&#39;utente che ha eseguito l&#39;accesso viene incluso nell&#39;account UserManager del coniglio. Una volta ottenuto l&#39;utente che ha effettuato l&#39;accesso, viene visualizzato il valore della proprietà accountNumber associata al profilo dell&#39;utente.

Quindi associamo il valore accountNumber a una chiave denominata accountnumber nella mappa. La chiave **accountnumber** è definita nella modale dei dati del modulo come Attributo richiesta. Il valore di questo attributo viene passato come parametro di input al metodo del servizio di lettura Form Data Modal.

Linea 7: La richiesta ricevuta viene inviata a un altro servlet, in base al tipo di risorsa identificato dall&#39;URL del documento di comunicazione interattiva. La risposta restituita da questo secondo servlet è inclusa nella risposta del primo servlet.

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

![requestParameter](assets/requestparameter.png)

Attributo di richiesta definito per il servizio di lettura della modale dei dati del modulo


[Esempio AEM pacchetto](assets/webchanneldelivery.zip).
