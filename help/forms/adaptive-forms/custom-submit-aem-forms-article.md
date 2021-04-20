---
title: Scrittura di un invio personalizzato in AEM Forms
seo-title: Scrittura di un invio personalizzato in AEM Forms
description: Modo semplice e rapido per creare un’azione di invio personalizzata per il modulo adattivo
seo-description: Modo semplice e rapido per creare un’azione di invio personalizzata per il modulo adattivo
feature: Adaptive Forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 3%

---


# Scrittura di un invio personalizzato in AEM Forms {#writing-a-custom-submit-in-aem-forms}

Modo semplice e rapido per creare un’azione di invio personalizzata per il modulo adattivo

Questo articolo illustra i passaggi necessari per creare un’azione di invio personalizzata per la gestione dell’invio di moduli adattivi.

* Accedi a crx
* Crea un nodo di tipo &quot;sling :folder&quot; sotto le app. Chiamiamo questo nodo CustomSubmitHelpx.
* Salva il nodo appena creato.
* Aggiungi le due seguenti proprietà al nodo appena creato
* NomeProprietà       | Valore proprietà
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* Salva le modifiche
* Crea un nuovo file denominato post.POST.jsp sotto il nodo CustomSubmitHelpx.Quando viene inviato un modulo adattivo, viene chiamato questo JSP. Puoi scrivere il codice JSP in base alle tue esigenze in questo file. Il codice seguente inoltra la richiesta al servlet.

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

* Crea un file denominato addfields .jsp sotto il nodo CustomSubmitHelpx . Questo file consente di accedere al documento firmato.
* Aggiungi il codice seguente a questo file

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Salva le modifiche

Ora inizierai a vedere &quot;CustomSubmitHelpx&quot; nelle azioni di invio del modulo adattivo come mostrato in questa immagine.

![Modulo adattivo con invio personalizzato](assets/capture-2.gif)

