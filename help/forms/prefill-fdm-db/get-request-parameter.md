---
title: Ottieni parametro richiesta
description: Accedere al parametro della richiesta del servizio di precompilazione di un modello di dati del modulo
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 40
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# Ottieni parametro richiesta

## Ottieni parametro empID

Il passaggio successivo consiste nell’accedere al parametro empID dall’URL. Il valore del parametro della richiesta empID viene quindi passato all&#39;operazione del servizio **_get_** del modello dati del modulo.
Ai fini del presente corso abbiamo creato e fornito quanto segue

* Modello di modulo adattivo denominato **_FDMDemo_**
* Componente Pagina denominato **_fdmdemo_**
* Incluso il nostro JSP personalizzato con il componente Pagina
* Associato il modello di modulo adattivo al componente pagina

In questo modo, il nostro codice nel codice jsp personalizzato verrà eseguito solo quando viene eseguito il rendering del modulo adattivo basato su questo modello personalizzato

* [Importa il pacchetto](assets/template-page-component.zip) utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Apri fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Rimuovere il commento dalle righe commentate.
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
>La chiave empID deve corrispondere al valore di binding del servizio get delle nuove entità

## Passaggi successivi

[Creare un modulo adattivo basato sul modello di dati del modulo](./create-adaptive-form.md)
