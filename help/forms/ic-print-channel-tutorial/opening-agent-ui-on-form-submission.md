---
title: Apertura dell'interfaccia utente agente all'invio POST
seo-title: Apertura dell'interfaccia utente agente all'invio POST
description: Questa è la parte 11 dell'esercitazione multistep per la creazione del primo documento di comunicazione interattiva per il canale di stampa. In questa parte, verrà avviata l'interfaccia agente ui per la creazione della corrispondenza ad hoc all'invio del modulo.
seo-description: Questa è la parte 11 dell'esercitazione multistep per la creazione del primo documento di comunicazione interattiva per il canale di stampa. In questa parte, verrà avviata l'interfaccia agente ui per la creazione della corrispondenza ad hoc all'invio del modulo.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---


# Apertura dell&#39;interfaccia utente agente all&#39;invio POST

In questa parte, verrà avviata l&#39;interfaccia agente ui per la creazione della corrispondenza ad hoc all&#39;invio del modulo.

Questo articolo illustra i passaggi necessari per aprire l&#39;interfaccia utente agente all&#39;invio di un modulo. Tipico esempio di utilizzo è che l&#39;agente del servizio clienti compila un modulo con alcuni parametri di input e che l&#39;agente di invio del modulo ui viene aperto con i dati precompilati dal servizio di precompilazione del modello di dati del modulo. I parametri di input per il servizio di precompilazione del modello di dati del modulo vengono estratti dall&#39;invio del modulo.

Il seguente video mostra i casi di utilizzo

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

Linea 1: Ottenete il numero di conto dal parametro request

Linea 2-8: Create la mappa dei parametri e impostate chiavi e valori appropriati per riflettere documentId,Random.

Linea 9-10: Creare un altro oggetto Mappa per mantenere il parametro di input definito nel modello dati modulo.

Linea 11: Impostate l’attributo slingRequest &quot;paramMap&quot;

Linea 12-13: Inoltra la richiesta al servlet

Per testare questa funzionalità sul server

* [Importate e installate le risorse correlate a questo articolo utilizzando il gestore pacchetti.](assets/launch-agent-ui.zip)
* [Login a configMgr](http://localhost:4502/system/console/configMgr)
* Ricerca _Adobe Filtro CSRF Granite_
* Aggiungere _/content/getprintchannel_ nei percorsi esclusi
* Salvare le modifiche.
* [Aprite POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Assicurarsi che la stringa passata a FormFieldRequestParameter sia documentId valido.(Linea 19).
* [Aprire la pagina](http://localhost:4502/content/OpenPrintChannel.html) Web, inserire il numero di conto e inviare il modulo.
* L&#39;interfaccia utente dell&#39;agente deve essere aperta con i dati precompilati specifici per il numero di conto immesso nel modulo.

>[!NOTE]
>
>Verificare che il parametro di input dell&#39;operazione Get del modello dati modulo sia associato all&#39;attributo Request denominato &quot;accountnumber&quot; per consentire il corretto funzionamento dell&#39;operazione. Se si modifica il nome del valore di binding con qualsiasi altro nome, assicurarsi che la modifica sia riflessa nella riga 25 del file POST.jsp

