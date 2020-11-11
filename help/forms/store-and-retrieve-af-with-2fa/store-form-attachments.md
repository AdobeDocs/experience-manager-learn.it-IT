---
title: Memorizzare gli allegati del modulo
description: Estrarre gli allegati del modulo e archiviarli in una nuova posizione nell'archivio CRX.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6537
thumbnail: 6537.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Memorizzare gli allegati del modulo

Quando si aggiungono allegati a un modulo adattivo, gli allegati vengono memorizzati in una posizione temporanea nell&#39;archivio CRX. Per poter utilizzare il caso, è necessario memorizzare gli allegati del modulo in una nuova posizione nell&#39;archivio CRX.

Il servizio OSGi viene creato per memorizzare gli allegati del modulo in una nuova posizione nell&#39;archivio CRX. Viene creata una nuova mappa file con la nuova posizione degli allegati nel CRX e viene restituita all’applicazione chiamante.
Di seguito è riportata la FileMap inviata al servlet. La chiave è il campo modulo adattivo e il valore è la posizione temporanea dell&#39;allegato. Nel nostro servlet estrarremo l&#39;allegato e lo archivieremo in una nuova posizione nell&#39;archivio AEM e aggiorneremo la FileMap con la nuova posizione

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

Di seguito è riportato il codice che estrae gli allegati dalla richiesta e li memorizza nella cartella **/content/afattachments** .

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

Si tratta della nuova Mappa file con la posizione aggiornata degli allegati del modulo

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```
