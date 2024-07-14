---
title: Apertura dell’interfaccia utente dell’agente all’invio del POST
description: Questa è la parte 11 di un tutorial a più passaggi per creare il primo documento di comunicazione interattiva per il canale di stampa. In questa parte verrà avviata l’interfaccia dell’interfaccia utente agente per la creazione di corrispondenza ad hoc all’invio del modulo.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 170
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Apertura dell’interfaccia utente dell’agente all’invio del POST

In questa parte verrà avviata l’interfaccia dell’interfaccia utente agente per la creazione di corrispondenza ad hoc all’invio del modulo.

Questo articolo illustra i passaggi necessari per aprire l’interfaccia utente agente all’invio di un modulo. In genere, l’agente del servizio clienti compila un modulo con alcuni parametri di input e all’invio del modulo l’interfaccia utente dell’agente viene aperta con dati precompilati dal servizio di precompilazione del modello di dati del modulo. I parametri di input per il servizio di precompilazione del modello di dati del modulo vengono estratti dall’invio del modulo.

Il seguente video mostra un caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

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

Riga 1 : ottiene il numero di conto dal parametro request

Riga 2-8: crea una mappa dei parametri e imposta le chiavi e i valori appropriati per riflettere documentId,Random.

Linea 9-10: crea un altro oggetto Map in cui inserire il parametro di input definito nel modello di dati del modulo.

Riga 11: imposta l’attributo slingRequest &quot;paramMap&quot;

Riga 12-13: inoltra la richiesta al servlet

Per testare questa funzionalità sul server

* [Importa e installa le risorse correlate a questo articolo utilizzando Gestione pacchetti.](assets/launch-agent-ui.zip)
* [Accesso a configMgr](http://localhost:4502/system/console/configMgr)
* Cerca _Filtro CSRF Adobe Granite_
* Aggiungi _/content/getprintchannel_ nei percorsi esclusi
* Salva le modifiche.
* [Apri POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Verificare che la stringa passata a FormFieldRequestParameter sia documentId valido.(riga 19).
* [Aprire la pagina Web](http://localhost:4502/content/OpenPrintChannel.html), immettere il numero di account e inviare il modulo.
* L’interfaccia utente dell’agente deve aprirsi con i dati precompilati specifici del numero account immesso nel modulo.

>[!NOTE]
>
>Assicurati che il parametro di input dell’operazione Get del modello dati modulo sia associato all’attributo di richiesta denominato &quot;accountnumber&quot; affinché questo funzioni. Se modifichi il nome del valore di binding con un altro nome, assicurati che la modifica si rifletta sulla riga 25 del file POST.jsp
