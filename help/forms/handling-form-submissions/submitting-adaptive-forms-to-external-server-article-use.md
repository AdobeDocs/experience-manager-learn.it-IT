---
title: Invio di un modulo adattivo a un server esterno
description: Invio di un modulo adattivo all’endpoint REST in esecuzione su un server esterno
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# Invio di un modulo adattivo a un server esterno {#submitting-adaptive-form-to-external-server}

Utilizza l’azione Invia a endpoint REST per inviare i dati inviati a un URL REST. L’URL può essere interno (il server sul quale viene eseguito il rendering del modulo) o esterno.

In genere, i clienti desiderano inviare i dati del modulo a un server esterno per l’ulteriore elaborazione.

Per pubblicare i dati su un server interno, specifica il percorso della risorsa. I dati vengono inseriti nel percorso della risorsa. Ad esempio: &lt;/content restendpoint=&quot;&quot;> . Per tali richieste successive, vengono utilizzate le informazioni di autenticazione della richiesta di invio.

Per pubblicare dati su un server esterno, fornisci un URL. Il formato dell’URL è <http://host:port/path_to_rest_end_point>. Verifica di aver configurato il percorso per gestire la richiesta POST in modo anonimo.

Ai fini di questo articolo, ho scritto un semplice file war che può essere distribuito sulla tua istanza tomcat. Se Tomcat è in esecuzione sulla porta 8080, l’URL POST sarà

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

quando configuri il modulo adattivo per l’invio a questo endpoint, i dati del modulo e gli eventuali allegati possono essere estratti nel servlet con il seguente codice

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

![formsubmit](assets/formsubmission.gif)
Per eseguire il test sul server, effettuare le seguenti operazioni

1. Installa Tomcat se non lo hai già. [Le istruzioni per installare tomcat sono disponibili qui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Scarica il file [file zip](assets/aemformsenablement.zip) associato a questo articolo. Decomprimi il file per ottenere il file .war.
1. Distribuire il file .war nel server Tomcat.
1. Crea un semplice modulo adattivo con il componente File allegato e configurane l’azione di invio come mostrato nella schermata precedente. L’URL del POST è <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Se l’AEM e tomcat non sono in esecuzione su localhost, modifica l’URL di conseguenza.
1. Per abilitare l’invio di dati da moduli multipart a tomcat, aggiungi il seguente attributo all’elemento di contesto del &lt;tomcatinstalldir>\conf\context.xml e riavviare il server Tomcat.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Visualizza l’anteprima del modulo adattivo, aggiungi un allegato e invia. Controlla la finestra della console Tomcat per i messaggi.
