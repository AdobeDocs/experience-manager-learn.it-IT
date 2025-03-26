---
title: AEM Forms con schema JSON e dati[Part4]
description: Tutorial in più parti per illustrare i passaggi necessari per creare un modulo adattivo con schema JSON e interrogare i dati inviati.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 99
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Query dei dati inviati


Il passaggio successivo consiste nell’eseguire una query sui dati inviati e visualizzare i risultati in formato tabulare. A tale scopo, utilizziamo il seguente software:

[QueryBuilder](https://querybuilder.js.org/) - Componente interfaccia utente per la creazione di query

[Tabelle dati](https://datatables.net/)- Per visualizzare i risultati della query sotto forma di tabella.

La seguente interfaccia utente è stata creata per abilitare l’esecuzione di query sui dati inviati. Solo gli elementi contrassegnati come obbligatori nello schema JSON sono disponibili per le query su. Nella schermata seguente, stiamo eseguendo una query per tutte le richieste in cui deliverypref è SMS.

L’interfaccia utente di esempio per eseguire query sui dati inviati non utilizza tutte le funzionalità avanzate disponibili in QueryBuilder. Ti invitiamo a provarli da solo.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La versione corrente di questa esercitazione non supporta l&#39;esecuzione di query su più colonne.

Quando si seleziona un modulo per eseguire la query, viene effettuata una chiamata GET a **/bin/getdatakeysfromschema**. Questa chiamata di GET restituisce i campi obbligatori associati allo schema dei moduli. I campi obbligatori vengono quindi inseriti nell&#39;elenco a discesa di QueryBuilder per consentire la creazione della query.

Il seguente frammento di codice effettua una chiamata al metodo getRequiredColumnsFromSchema del servizio JSONSchemaOperations. Trasmettiamo le proprietà e gli elementi richiesti dello schema a questa chiamata del metodo. L’array restituito da questa chiamata di funzione viene quindi utilizzato per popolare l’elenco a discesa Query Builder

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

Quando si fa clic sul pulsante GetResult, viene effettuata una chiamata Get a **&quot;/bin/querydata&quot;**. La query generata dall&#39;interfaccia utente di QueryBuilder viene passata al servlet tramite il parametro query. Il servlet massaggia quindi questa query in una query SQL che può essere utilizzata per eseguire query sul database. Ad esempio, se si cerca di recuperare tutti i prodotti denominati &#39;Mouse&#39;, la stringa di query di Query Builder è `$.productname = 'Mouse'`. Questa query verrà quindi convertita nel seguente

SELEZIONA &#42; da aemformswithjson.  formsubmissions dove JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

Il risultato di questa query viene quindi restituito per popolare la tabella nell’interfaccia utente.

Per eseguire questo esempio sul sistema locale, attenersi alla seguente procedura

1. [Assicurarsi di aver seguito tutti i passaggi qui indicati](part2.md)
1. [Importa il file Dashboardv2.zip utilizzando Gestione pacchetti di AEM.](assets/dashboardv2.zip) Questo pacchetto contiene tutti i bundle necessari, le impostazioni di configurazione, l&#39;invio personalizzato e la pagina di esempio per eseguire query sui dati.
1. Creare un modulo adattivo utilizzando lo schema json di esempio
1. Configurare il modulo adattivo per l’invio all’azione di invio personalizzata &quot;customsubmithelpx&quot;
1. Compila il modulo e invia
1. Scegli il browser per [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Selezionare il modulo ed eseguire una query semplice
