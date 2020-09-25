---
title: Scrittura di un invio personalizzato in  AEM Forms
seo-title: Scrittura di un invio personalizzato in  AEM Forms
description: Modalità semplice e rapida per creare un'azione di invio personalizzata per il modulo adattivo
seo-description: Modalità semplice e rapida per creare un'azione di invio personalizzata per il modulo adattivo
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 1%

---


# Scrittura di un invio personalizzato in  AEM Forms {#writing-a-custom-submit-in-aem-forms}

Modalità semplice e rapida per creare un&#39;azione di invio personalizzata per il modulo adattivo

Questo articolo illustra i passaggi necessari per creare un&#39;azione di invio personalizzata per gestire l&#39;invio di Forms adattivo.

* Login a crx
* Create un nodo di tipo &quot;sling :folder&quot; sotto le app. Chiamiamo questo nodo CustomSubmitHelpx.
* Salvare il nodo appena creato.
* Aggiungi le due seguenti proprietà al nodo appena creato
* PropertyName       | Valore proprietà
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* Salva le modifiche
* Creare un nuovo file denominato post.POST.jsp sotto il nodo CustomSubmitHelpx. Quando viene inviato un modulo adattivo, viene chiamato questo JSP. È possibile scrivere il codice JSP in base alle proprie esigenze in questo file. Il codice seguente inoltra la richiesta al servlet.

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

* Create un file denominato addfields .jsp sotto il nodo CustomSubmitHelpx. Questo file consente di accedere al documento firmato.
* Aggiungi il seguente codice a questo file

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Salvare le modifiche

Ora verrà visualizzato &quot;CustomSubmitHelpx&quot; nelle azioni di invio del modulo adattivo, come illustrato in questa immagine.

![Modulo adattivo con invio personalizzato](assets/capture-2.gif)

