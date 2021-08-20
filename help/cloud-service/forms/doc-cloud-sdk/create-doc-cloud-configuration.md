---
title: Creare una configurazione OSGi personalizzata
description: Configurazione OSGi personalizzata per acquisire i dettagli specifici di document cloud
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Sviluppo
thumbnail: 7818.jpg
kt: 7818
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '64'
ht-degree: 3%

---

# Introduzione

Crea una configurazione OSGi personalizzata per acquisire le credenziali dell’account cloud del documento


Per creare una configurazione OSGi personalizzata, è necessario innanzitutto creare un’interfaccia i cui metodi pubblici rappresentino i campi nella configurazione.

![doc-cloud-config](assets/doc-cloud-configuration.JPG)


Crea un&#39;interfaccia denominata DocumentCloudConfiguration e incolla in essa il seguente codice.

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```
