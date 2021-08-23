---
title: AEM Forms con schema e dati JSON[Parte4]
seo-title: AEM Forms con schema e dati JSON[Parte4]
description: Esercitazione in più parti per illustrarti i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
seo-description: Esercitazione in più parti per illustrarti i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
feature: Moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Query dei dati inviati


Il passaggio successivo consiste nell’interrogare i dati inviati e visualizzare i risultati in modo tabulare. A questo scopo utilizzeremo il seguente software

[QueryBuilder](https://querybuilder.js.org/) : componente dell’interfaccia utente per creare query

[Tabelle dati](https://datatables.net/): per visualizzare i risultati della query in modo tabulare.

È stata creata la seguente interfaccia utente per abilitare la query dei dati inviati. Solo gli elementi contrassegnati come necessari nello schema JSON sono resi disponibili per la query. Nella schermata seguente, stiamo eseguendo una query per tutti gli invii in cui il deliverypref è SMS.

L’interfaccia utente di esempio per eseguire una query sui dati inviati non utilizza tutte le funzionalità avanzate disponibili in QueryBuilder. Siete incoraggiati a provarli da soli.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La versione corrente di questa esercitazione non supporta la query su più colonne.

Quando selezioni un modulo per eseguire la query, viene effettuata una chiamata GET a **/bin/getdatakeysfromschema**. Questa chiamata di GET restituisce i campi obbligatori associati allo schema dei moduli. I campi obbligatori vengono quindi compilati nell’elenco a discesa di QueryBuilder per consentire la creazione della query.

Lo snippet di codice seguente effettua una chiamata al metodo getRequiredColumnsFromSchema del servizio JSONSchemaOperations. Trasmettiamo le proprietà e gli elementi richiesti dello schema a questa chiamata del metodo . L&#39;array restituito da questa chiamata di funzione viene quindi utilizzato per compilare l&#39;elenco a discesa del generatore di query

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

Quando si fa clic sul pulsante GetResult, viene effettuata una chiamata Get a **&quot;/bin/querydata&quot;**. Trasmettiamo la query generata dall’interfaccia utente di QueryBuilder al servlet attraverso il parametro di query. Il servlet quindi massaggia questa query nella query SQL che può essere utilizzata per eseguire query sul database. Ad esempio, se stai cercando di recuperare tutti i prodotti denominati &#39;Mouse&#39;, la stringa di query di Query Builder sarà $.productname = &#39;Mouse&#39;. Questa query verrà quindi convertita nel seguente

SELECT * from aemformswithjson .  moduli inviati in cui JSON_EXTRACT( moduli inviati .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

Il risultato di questa query viene quindi restituito per compilare la tabella nell’interfaccia utente.

Per eseguire questo esempio sul sistema locale, esegui i seguenti passaggi

1. [Assicurati di aver seguito tutti i passaggi indicati qui](part2.md)
1. [Importa il file Dashboardv2.zip utilizzando AEM Package Manager.](assets/dashboardv2.zip) Questo pacchetto contiene tutti i bundle, le impostazioni di configurazione, l&#39;invio personalizzato e la pagina di esempio necessari per eseguire query sui dati.
1. Creare un modulo adattivo utilizzando lo schema json di esempio
1. Configurare il modulo adattivo per l’invio all’azione di invio personalizzata &quot;customsubmithelpx&quot;
1. Compila il modulo e invia
1. Posiziona il browser su [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Selezionare il modulo ed eseguire query semplici

