---
title: ' AEM Forms con schema e dati JSON[Part4]'
seo-title: ' AEM Forms con schema e dati JSON[Part4]'
description: Esercitazione con più parti per illustrare i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
seo-description: Esercitazione con più parti per illustrare i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# Query dei dati inviati


Il passaggio successivo consiste nell&#39;eseguire una query sui dati inviati e visualizzare i risultati in modo tabulare. A tal fine, utilizzeremo il seguente software

[QueryBuilder](https://querybuilder.js.org/)  - Componente interfaccia utente per creare query

[Tabelle](https://datatables.net/) dati - Per visualizzare i risultati della query in modo tabulare.

La seguente interfaccia utente è stata creata per abilitare la query dei dati inviati. Solo gli elementi contrassegnati come obbligatori nello schema JSON sono messi a disposizione per la query. Nella schermata sottostante, stiamo interrogando per tutti gli invii in cui il recapito pref è SMS.

L&#39;interfaccia utente di esempio per la query dei dati inviati non utilizza tutte le funzionalità avanzate disponibili in QueryBuilder. Siete incoraggiati a provarli da soli.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La versione corrente di questa esercitazione non supporta l&#39;esecuzione di query su più colonne.

Quando si seleziona un modulo per eseguire la query, viene effettuata una chiamata di GET a **/bin/getdatakeysfromschema**. Questa chiamata di GET restituisce i campi obbligatori associati allo schema dei moduli. I campi obbligatori vengono quindi compilati nell&#39;elenco a discesa di QueryBuilder per consentire all&#39;utente di creare la query.

Lo snippet di codice seguente esegue una chiamata al metodo getRequiredColumnsFromSchema del servizio JSONSchemaOperations. Trasmettiamo le proprietà e gli elementi richiesti dello schema a questa chiamata del metodo. L&#39;array restituito da questa chiamata di funzione viene quindi utilizzato per compilare l&#39;elenco a discesa del generatore di query

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

Quando si fa clic sul pulsante GetResult, viene effettuata una chiamata Get a **&quot;/bin/querydata&quot;**. Trasmettiamo la query creata dall&#39;interfaccia utente di QueryBuilder al servlet attraverso il parametro query. Il servlet quindi massaggia la query nella query SQL che può essere utilizzata per eseguire una query sul database. Ad esempio, se state cercando di recuperare tutti i prodotti denominati &#39;Mouse&#39;, la stringa di query di Query Builder sarà $.productname = &#39;Mouse&#39;. La query verrà quindi convertita nel seguente

SELECT * from aemformswithjson .  formsubmit where JSON_EXTRACT( forminvii .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

Il risultato di questa query viene quindi restituito per compilare la tabella nell&#39;interfaccia utente.

Per eseguire questo esempio sul sistema locale, eseguire i seguenti passaggi

1. [Accertatevi di aver seguito tutti i passaggi indicati qui](part2.md)
1. [Importa il file Dashboardv2.zip tramite AEM Package Manager.](assets/dashboardv2.zip) Questo pacchetto contiene tutti i bundle, le impostazioni di configurazione, l&#39;invio personalizzato e la pagina di esempio necessari per eseguire la query sui dati.
1. Creare un modulo adattivo utilizzando lo schema json di esempio
1. Configurare il modulo adattivo per l&#39;invio all&#39;azione di invio personalizzata &quot;customsubmithelpx&quot;
1. Compilare il modulo e inviare
1. Posizionare il browser su [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Selezionare il modulo ed eseguire una semplice query

