---
title: Ottieni parametro di richiesta
description: Accedere al parametro di richiesta del servizio di precompilazione di un modello di dati modulo
feature: moduli adattivi
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

---

# Ottieni parametro di richiesta

## Ottieni il parametro empID

Il passaggio successivo consiste nell’accedere al parametro empID dall’url. Il valore del parametro di richiesta empID viene quindi passato all&#39;operazione del servizio **_get_** del modello di dati del modulo.
Per questo corso abbiamo creato e fornito quanto segue

* Modello di modulo adattivo denominato **_FDMDemo_**
* Componente pagina denominato **_fdmdemo_**
* Incluso il nostro jsp personalizzato con il componente pagina
* È stato associato il modello di modulo adattivo al componente pagina

In questo modo il nostro codice nel jsp personalizzato verrà eseguito solo quando viene eseguito il rendering di un modulo adattivo basato su questo modello personalizzato

* [Importa il ](assets/template-page-component.zip) pacchetto utilizzando il gestore  [dei pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Apri fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Rimuovi il commento dalle righe commentate.
* Salva le modifiche

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

Il valore di empID è associato alla chiave denominata empID in paraMap. Questa mappa viene quindi passata a slingRequest

>[!NOTE]
>
>L&#39;empID chiave deve corrispondere al valore di binding del servizio get entità successive
