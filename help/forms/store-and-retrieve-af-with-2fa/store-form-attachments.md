---
title: Memorizza allegati modulo
description: Estrarre gli allegati del modulo e archiviarli in una nuova posizione nell’archivio CRX.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
duration: 64
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---

# Memorizza allegati modulo

Quando si aggiungono allegati a un modulo adattivo, gli allegati vengono memorizzati in una posizione temporanea nell’archivio CRX. Affinché il caso d’uso funzioni, è necessario memorizzare gli allegati del modulo in una nuova posizione nell’archivio CRX.

Il servizio OSGi viene creato per memorizzare gli allegati del modulo in una nuova posizione nell’archivio CRX. Viene creata una nuova mappa file con la nuova posizione degli allegati nel CRX e viene restituita all’applicazione chiamante.
Di seguito è riportato il FileMap inviato al servlet. La chiave è il campo del modulo adattivo e il valore è la posizione temporanea dell’allegato. Nel nostro servlet estraeremo l’allegato e lo memorizzeremo in una nuova posizione nell’archivio AEM e aggiorneremo FileMap con la nuova posizione

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

Di seguito è riportato il codice che estrae gli allegati dalla richiesta e li memorizza in **/content/afattachments** cartella

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

Nuovo FileMap con la posizione aggiornata degli allegati del modulo

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## Passaggi successivi

[Salvare i dati del modulo](./store-form-data.md)