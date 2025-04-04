---
title: Crea nuovo componente pagina
description: Creare un nuovo componente pagina
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 7469aa7f-1794-40dd-990c-af5d45e85223
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 8%

---

# Componente Pagina 

Un componente pagina è un componente normale responsabile del rendering di una pagina. Stiamo per creare un nuovo componente pagina e associare questo componente pagina a un nuovo modello di modulo adattivo. In questo modo il nostro codice verrà eseguito solo quando un modulo adattivo è basato su questo particolare modello.

## Crea componente pagina

Accedi all’istanza AEM Forms locale predisposta per il cloud. Crea la seguente struttura nella cartella delle app
![componente-pagina](./assets/page-component1.png)

1. Fai clic con il pulsante destro del mouse sulla cartella della pagina e crea un nodo denominato storeandfetch di tipo cq:Component
1. Salva le modifiche
1. Aggiungi le seguenti proprietà al nodo `storeandfetch` e salva

| **Nome proprietà** | **Tipo di proprietà** | **Valore proprietà** |
|-------------------------|-------------------|----------------------------------------|
| componentGroup | Stringa | nascosto |
| jcr:descrizione | Stringa | Tipo pagina modello modulo adattivo |
| jcr:title | Stringa | Pagina modello modulo adattivo |
| sling:resourceSuperType | Stringa | `fd/af/components/page2/aftemplatedpage` |

Copiare `/libs/fd/af/components/page2/aftemplatedpage/aftemplatedpage.jsp` e incollarlo nel nodo `storeandfetch`. Rinomina `aftemplatedpage.jsp` in `storeandfetch.jsp`.

Apri `storeandfetch.jsp` e aggiungi la seguente riga:

```jsp
<cq:include script="azureportal.jsp"/>
```

sotto

```jsp
<cq:include script="fallbackLibrary.jsp"/>
```

Il codice finale deve essere simile al seguente

```jsp
<cq:include script="fallbackLibrary.jsp"/>
<cq:include script="azureportal.jsp"/>
```

Crea un file denominato azureportal.jsp sotto il nodo storeandfetch
copia il seguente codice in azureportal.jsp e salva le modifiche

```jsp
<%@page session="false" %>
<%@include file="/libs/fd/af/components/guidesglobal.jsp" %>
<%@ page import="org.apache.commons.logging.Log" %>
<%@ page import="org.apache.commons.logging.LogFactory" %>
<%
    if(request.getParameter("guid")!=null) {
            logger.debug( "Got Guid in the request" );
            String BlobId = request.getParameter("guid");
            java.util.Map paraMap = new java.util.HashMap();
            paraMap.put("BlobId",BlobId);
            slingRequest.setAttribute("paramMap",paraMap);
    } else {
            logger.debug( "There is no Guid in the request " );
    }            
%>
```

In questo codice si ottiene il valore del parametro di richiesta **guid** e lo si archivia in una variabile denominata BlobId. Questo BlobId viene quindi trasmesso nella richiesta sling utilizzando l’attributo paramMap. Affinché questo codice funzioni, si presume che sia presente un modulo basato su un modello di dati del modulo supportato dall’archiviazione di Azure e che il servizio di lettura del modello di dati del modulo sia associato a un attributo di richiesta denominato BlobId, come illustrato nella schermata seguente.

![fdm-request-attribute](./assets/fdm-request-attribute.png)

### Passaggi successivi

[Associare il componente Pagina al modello](./associate-page-component.md)
