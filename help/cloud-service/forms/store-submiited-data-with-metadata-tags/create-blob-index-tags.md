---
title: Tutorial per aggiungere i tag di metadati specificati dall’utente
description: Crea un invio personalizzato per memorizzare i dati del modulo con i tag di metadati in Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 40
exl-id: eb9bcd1b-c86f-4894-bcf8-9c17e74dd6ec
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 1%

---

# Aggiungi tag indice BLOB

Con l’aumento delle dimensioni dei set di dati, può essere difficile trovare un oggetto specifico in un mare di dati. I tag di indice BLOB forniscono funzionalità di gestione e individuazione dei dati utilizzando gli attributi dei tag di indice chiave-valore. È possibile categorizzare e trovare oggetti all&#39;interno di un singolo contenitore o in tutti i contenitori nell&#39;account di archiviazione. Ad esempio, tag indice BLOB _**CustomerType=Platino**_, dove Platinum è il valore del campo CustomerType.

![index-tags](assets/blob-with-index-tags1.png)
Il codice seguente crea la stringa dei tag dei dati dell&#39;indice BLOB con i valori corrispondenti dai dati inviati

```java
@Override
    public String getMetaDataTags(String submittedFormName,String formPath,Session session,String formData) {

        JsonObject jsonObject = JsonParser.parseString(formData).getAsJsonObject();
        List<String>metaDataTags = new ArrayList<String>();
        metaDataTags.add("formName="+submittedFormName);
        Map< String, String > map = new HashMap< String, String >();
        map.put("path", formPath);
        map.put("1_property", "Searchable");
        map.put("1_property.value","true");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map),session);
        query.setStart(0);
        query.setHitsPerPage(20);
        SearchResult result = query.getResult();
        logger.debug("Get result hits " + result.getHits().size());
        for (Hit hit: result.getHits()) {
            try {
                    logger.debug(hit.getPath());
                    String jsonElementName = (String) hit.getProperties().get("name");
                    String fieldName = hit.getProperties().get("name").toString();
                    if(jsonObject.get(jsonElementName).isJsonArray())
                    {
                        
                        JsonArray arrayOfValues = jsonObject.get(jsonElementName).getAsJsonArray();
                        StringBuilder valuesString = new StringBuilder();
                        for(int j=0;j<arrayOfValues.size();j++)
                        {
                            valuesString.append(arrayOfValues.get(j).getAsString());
                            if(j < arrayOfValues.size() -1)
                            {
                                valuesString.append(" and ");
                            }
                        }

                        
                        metaDataTags.add(fieldName + "=" + valuesString.toString());

                    }
                    else
                    {
                        logger.debug("The searchable field name is " + fieldName + "the json element name is " + jsonElementName);
                        metaDataTags.add(fieldName + "=" + jsonObject.get(jsonElementName).getAsString());
                    }

            } catch (RepositoryException e) {
                throw new RuntimeException(e);
            }
        }
        return String.join("&",metaDataTags);
    }
```

## Passaggi successivi

[Creare un gestore di invio personalizzato](./create-custom-submit.md)
