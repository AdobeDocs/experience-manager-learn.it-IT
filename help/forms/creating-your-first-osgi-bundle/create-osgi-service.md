---
title: Creazione del primo servizio OSGi con AEM Forms
description: Crea il tuo primo servizio OSGi con AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 2%

---

# Servizio OSGi

Un servizio OSGi è un&#39;interfaccia di classe o servizio Java, insieme a una serie di proprietà del servizio come coppie nome/valore. Le proprietà del servizio differenziano tra i diversi provider di servizi che forniscono servizi con la stessa interfaccia di servizio.

Un servizio OSGi viene definito semanticamente dalla relativa interfaccia di servizio e implementato come oggetto di servizio. La funzionalità di un servizio è definita dalle interfacce che implementa. Pertanto, applicazioni diverse possono implementare lo stesso servizio. Le interfacce di servizio consentono ai bundle di interagire mediante interfacce di binding, non tramite implementazioni. Specificare un&#39;interfaccia di servizio con il minor numero possibile di dettagli di implementazione.

## Definire l’interfaccia

Interfaccia semplice con un metodo per unire i dati con <span class="x x-first x-last">XDP</span> modello.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Implementare l’interfaccia

Crea un nuovo pacchetto denominato `com.mysite.samples.impl` per mantenere l’implementazione dell’interfaccia.

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

Annotazione `@Component(...)` alla riga 10 marca questa classe Java come componente OSGi e la registra come servizio OSGi.

La `@Reference` annotazione fa parte dei servizi dichiarativi OSGi e viene utilizzata per inserire un riferimento [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) nella variabile `outputService`.


## Creare e distribuire il bundle

* Apri **finestra del prompt dei comandi**
* Accedi a `c:\aemformsbundles\mysite\core`
* Esegui il comando `mvn clean install -PautoInstallBundle`
* Il comando precedente genera e distribuisce automaticamente il bundle nella tua istanza AEM in esecuzione su localhost:4502

Il bundle sarà disponibile anche nella seguente posizione `C:\AEMFormsBundles\mysite\core\target`. Il bundle può anche essere distribuito in AEM utilizzando il [Console web Felix.](http://localhost:4502/system/console/bundles)

## Utilizzo del servizio

Ora puoi utilizzare il servizio nella pagina JSP. Il seguente frammento di codice mostra come accedere al servizio e utilizzare i metodi implementati dal servizio

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

Il pacchetto di esempio contenente la pagina JSP può essere [scaricato da qui](assets/learning_aem_forms.zip)

[Il bundle completo è disponibile per il download](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## Test del pacchetto

Importa e installa il pacchetto in AEM utilizzando il [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)

Utilizza un postman per effettuare una chiamata POST e fornire i parametri di input come mostrato nella schermata sottostante
![postino](assets/test-service-postman.JPG)
