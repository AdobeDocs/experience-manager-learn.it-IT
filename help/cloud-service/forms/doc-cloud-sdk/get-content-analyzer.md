---
title: Creare un’analisi del contenuto
description: Crea la parte JSON contenente le informazioni sui parametri di input per la chiamata REST.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Sviluppo
thumbnail: 7836.jpg
kt: 7836
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '59'
ht-degree: 5%

---


# Creare richieste di Analyzer

Crea un frammento JSON che definisce:

+ input
+ parameters
+ output.

I dettagli di questo parametro del modulo [sono disponibili qui.](https://documentcloud.adobe.com/document-services/index.html#post-createPDF)

Il codice di esempio riportato di seguito genera il frammento JSON per tutti i tipi di documenti di Office 365.

```java
package com.aemforms.doccloud.core.impl;

import com.google.gson.JsonObject;

public class GetContentAnalyser {
	public static String getContentAnalyserRequest(String fileName)
	{
		JsonObject outerWrapper = new JsonObject();
		
		
		JsonObject documentIn = new JsonObject();
		documentIn.addProperty("cpf:location", "InputFile0");

		if(fileName.endsWith(".pptx"))
		{
			documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.presentationml.presentation");
		}

		if(fileName.endsWith(".docx"))
		{
			
			documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.wordprocessingml.document");
		}
		if(fileName.endsWith(".xlsx"))
		{
			documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
		}
		if(fileName.endsWith(".xml"))
		{
			documentIn.addProperty("dc:format","text/plain");
		}
		if(fileName.endsWith(".jpg"))
		{
			documentIn.addProperty("dc:format","image/jpeg");
		}

		JsonObject cpfinputs = new JsonObject();
		cpfinputs.add("documentIn", documentIn);
		
		
		//documentInWrapper.add("documentIn",documentIn);
		JsonObject cpfengine = new JsonObject();
		cpfengine.addProperty("repo:assetId", "urn:aaid:cpf:Service-1538ece812254acaac2a07799503a430");
		
		JsonObject documentOut = new JsonObject();
		
		documentOut.addProperty("cpf:location", "multipartLabelOut");
		documentOut.addProperty("dc:format", "application/pdf");
		JsonObject documentOutWrapper = new JsonObject();
		documentOutWrapper.add("documentOut",documentOut);
	
		outerWrapper.add("cpf:inputs",cpfinputs);
		outerWrapper.add("cpf:engine", cpfengine);
		outerWrapper.add("cpf:outputs", documentOutWrapper);
		System.out.println("The content Analyser is "+outerWrapper.toString());
		
		return outerWrapper.toString();
		
		
		
		
		
	}

}
```
