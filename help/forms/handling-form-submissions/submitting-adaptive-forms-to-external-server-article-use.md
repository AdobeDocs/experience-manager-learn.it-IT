---
title: Invio del modulo adattivo al server esterno
seo-title: Invio del modulo adattivo al server esterno
description: Invio del modulo adattivo all'endpoint REST in esecuzione su server esterno
seo-description: Invio del modulo adattivo all'endpoint REST in esecuzione su server esterno
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---


# Invio del modulo adattivo al server esterno {#submitting-adaptive-form-to-external-server}

Utilizzare l&#39;azione Invia a endpoint REST per inviare i dati inviati a un URL REST. L&#39;URL può essere di un server interno (il server su cui viene eseguito il rendering del modulo) o di un server esterno.

In genere, i clienti desiderano inviare i dati del modulo a un server esterno per un&#39;ulteriore elaborazione.

Per inviare i dati a un server interno, specificare il percorso della risorsa. I dati vengono inviati nel percorso della risorsa. Ad esempio, &lt;/content/restEndPoint> . Per tali richieste di post, vengono utilizzate le informazioni di autenticazione della richiesta di invio.

Per inviare dati a un server esterno, immetti un URL. The format of the URL is <http://host:port/path_to_rest_end_point>. Verificate di aver configurato il percorso per gestire la richiesta di POST in modo anonimo.

Per lo scopo di questo articolo, ho scritto un semplice file di guerra che può essere distribuito sulla vostra istanza tomcat. Se il vostro gatto è in esecuzione sulla porta 8080, l&#39;URL POST sarà

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

quando si configura il modulo adattivo per l&#39;invio a questo endpoint, i dati del modulo e gli eventuali allegati possono essere estratti nel servlet dal seguente codice

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![form:submit](assets/formsubmission.gif)Per eseguire il test sul server, effettuare le seguenti operazioni

1. Installate Tomcat se non lo avete già. [Le istruzioni per installare tomcat sono disponibili qui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Scaricate il file [](assets/aemformsenablement.zip) zip associato a questo articolo. Decomprimete il file per ottenere il file di guerra.
1. Distribuire il file di guerra nel server Tomcat.
1. Crea un semplice modulo adattivo con un componente per l’allegato del file e configurane l’azione di invio come mostrato nella schermata precedente. L&#39;URL POST è <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Se il AEM e il gatto non sono in esecuzione su localhost, modificare l&#39;URL di conseguenza.
1. Per abilitare l&#39;invio di dati del modulo multiparte a tomcat, aggiungere l&#39;attributo seguente all&#39;elemento contestuale di &lt;tomcatInstallDir>\conf\context.xml e riavviare il server Tomcat.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Visualizzare l’anteprima del modulo adattivo, aggiungere un allegato e inviare il modulo. Controlla se sono presenti messaggi nella finestra della console Tomcat.

