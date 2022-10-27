---
title: Invio del modulo adattivo al server esterno
seo-title: Submitting Adaptive Form to External Server
description: Invio del modulo adattivo all’endpoint REST in esecuzione su server esterno
seo-description: Submitting Adaptive Form to REST endpoint running on external server
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Invio del modulo adattivo al server esterno {#submitting-adaptive-form-to-external-server}

Utilizzare l’azione Invia a endpoint REST per inviare i dati inviati a un URL REST. L’URL può essere di un server interno (il server su cui viene eseguito il rendering del modulo) o di un server esterno.

In genere, i clienti desiderano inviare i dati del modulo a un server esterno per un’ulteriore elaborazione.

Per inviare dati a un server interno, fornisci un percorso della risorsa. I dati vengono inviati nel percorso della risorsa. Ad esempio: &lt;/content restendpoint=&quot;&quot;> . Per tali richieste post, vengono utilizzate le informazioni di autenticazione della richiesta di invio.

Per inviare dati a un server esterno, specifica un URL. Il formato dell’URL è <http://host:port/path_to_rest_end_point>. Verifica di aver configurato il percorso per gestire la richiesta POST in modo anonimo.

Per lo scopo di questo articolo, ho scritto un semplice file di guerra che può essere distribuito sulla tua istanza tomcat. Supponendo che il tuo gatto sia in esecuzione sulla porta 8080, l&#39;url di POST sarà

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

quando si configura il modulo adattivo per l’invio a questo endpoint, i dati del modulo e gli eventuali allegati possono essere estratti nel servlet dal seguente codice

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

![modulo](assets/formsubmission.gif)
Per eseguire il test sul server, effettua le seguenti operazioni

1. Installa Tomcat se non lo hai già. [Le istruzioni per l&#39;installazione di tomcat sono disponibili qui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Scarica la [file zip](assets/aemformsenablement.zip) associato a questo articolo. Decomprimere il file per ottenere il file WAR.
1. Distribuisci il file war nel server tomcat.
1. Crea un modulo adattivo semplice con un componente file allegato e configurane l’azione di invio come mostrato nella schermata precedente. L’URL di POST è <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Se il tuo AEM e tomcat non sono in esecuzione su localhost, modifica l&#39;URL di conseguenza.
1. Per abilitare l’invio di dati modulo multiparte a tomcat, aggiungere il seguente attributo all’elemento contestuale del &lt;tomcatinstalldir>\conf\context.xml e riavvia il server Tomcat.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Visualizzare in anteprima il modulo adattivo, aggiungere un allegato e inviare. Controlla la finestra della console tomcat per i messaggi.
