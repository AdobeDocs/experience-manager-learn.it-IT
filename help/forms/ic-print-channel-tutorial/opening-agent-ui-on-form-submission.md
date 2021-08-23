---
title: Apertura Dell’Interfaccia Utente Dell’Agente All’Invio Di POST
seo-title: Apertura Dell’Interfaccia Utente Dell’Agente All’Invio Di POST
description: Questa è la parte 11 dell'esercitazione su più passaggi per creare il primo documento di comunicazione interattivo per il canale di stampa. In questa parte verrà avviata l’interfaccia dell’interfaccia dell’agente per la creazione di corrispondenza ad-hoc all’invio del modulo.
seo-description: Questa è la parte 11 dell'esercitazione su più passaggi per creare il primo documento di comunicazione interattivo per il canale di stampa. In questa parte verrà avviata l’interfaccia dell’interfaccia dell’agente per la creazione di corrispondenza ad-hoc all’invio del modulo.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Comunicazione interattiva
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: Sviluppo
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---


# Apertura Dell’Interfaccia Utente Dell’Agente All’Invio Di POST

In questa parte verrà avviata l’interfaccia dell’interfaccia dell’agente per la creazione di corrispondenza ad-hoc all’invio del modulo.

Questo articolo illustra i passaggi necessari per aprire l’interfaccia utente dell’agente dopo l’invio di un modulo. Un caso d’uso tipico è che l’agente del servizio clienti compila un modulo con alcuni parametri di input e che l’interfaccia utente dell’agente di invio del modulo si apra con i dati precompilati dal servizio di precompilazione del modello di dati del modulo. I parametri di input per il servizio di precompilazione del modello di dati del modulo vengono estratti dall’invio del modulo.

Il seguente video mostra il caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

Linea 1 : Ottieni il numero di account dal parametro requestparameter

Linea 2-8: Crea la mappa dei parametri e imposta le chiavi e i valori appropriati per riflettere il documentId, Random.

Linea 9-10: Creare un altro oggetto Map contenente il parametro di input definito nel modello dati modulo.

Linea 11: Imposta l&#39;attributo slingRequest &quot;paramMap&quot;

Linea 12-13: Inoltra la richiesta al servlet

Per testare questa funzionalità sul server

* [Importa e installa le risorse correlate a questo articolo utilizzando il gestore di pacchetti.](assets/launch-agent-ui.zip)
* [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
* Cerca _Filtro CSRF Granite Adobe_
* Aggiungi _/content/getprintchannel_ nei percorsi esclusi
* Salva le modifiche.
* [Apri POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Assicurati che la stringa passata a FormFieldRequestParameter sia valida documentId.(Linea 19).
* [Aprire la pagina ](http://localhost:4502/content/OpenPrintChannel.html) Web, inserire il numero di conto e inviare il modulo.
* L’interfaccia utente dell’agente deve essere aperta con i dati precompilati specifici per il numero di account immesso nel modulo.

>[!NOTE]
>
>Assicurati che il parametro di input dell&#39;operazione Get del modello dati del modulo sia associato all&#39;attributo di richiesta denominato &quot;numero di conto&quot; affinché questo funzioni. Se modifichi il nome del valore di binding con qualsiasi altro nome, accertati che la modifica si rifletta sulla riga 25 di POST.jsp

