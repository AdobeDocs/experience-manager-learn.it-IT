---
title: Scrittura di un invio personalizzato in AEM Forms
description: Modo rapido e semplice per creare un’azione di invio personalizzata per il modulo adattivo
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
duration: 51
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 1%

---

# Scrittura di un invio personalizzato in AEM Forms {#writing-a-custom-submit-in-aem-forms}

Modo rapido e semplice per creare un’azione di invio personalizzata per il modulo adattivo

>[!NOTE]
>Il codice di questo articolo non funziona con un modulo adattivo basato su componenti core.
>[L&#39;articolo equivalente per il modulo adattivo basato su componenti core è disponibile qui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/custom-submit-headless-forms/custom-submit-service.html?lang=en)


Questo articolo illustra i passaggi necessari per creare un’azione di invio personalizzata per la gestione dell’invio di Forms adattivi.

* Accedi a crx
* Crea un nodo di tipo &quot;sling :folder &quot; in app. Chiamiamo questo nodo CustomSubmitHelpx.
* Salva il nodo appena creato.
* Aggiungi le tre proprietà seguenti al nodo appena creato

| Nome proprietà | Valore proprietà |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidesubmittype |
| guideDataModel | xfa,xsd,base |
| jcr :description | CustomSubmitHelpx |


* Salva le modifiche
* Crea un nuovo file denominato post.POST.jsp sotto il nodo CustomSubmitHelpx. Quando viene inviato un modulo adattivo, viene chiamato questo JSP. Puoi scrivere il codice JSP in base alle tue esigenze in questo file. Il codice seguente inoltra la richiesta al servlet.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* Crea un file denominato addfields .jsp sotto il nodo CustomSubmitHelpx. Questo file consente di accedere al documento firmato.
* Aggiungi il codice seguente al file

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Salva le modifiche

Ora inizierai a visualizzare &quot;CustomSubmitHelpx&quot; nelle azioni di invio del modulo adattivo, come illustrato in questa immagine.

![Modulo adattivo con invio personalizzato](assets/capture-2.gif)
