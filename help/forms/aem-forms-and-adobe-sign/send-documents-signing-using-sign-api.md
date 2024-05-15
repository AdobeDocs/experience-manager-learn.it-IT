---
title: Utilizzo delle API di Adobe Sign in AEM Forms
description: Inviare documenti per la firma utilizzando i metodi helper di Adobe Sign
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Utilizzo dei metodi helper di Adobe Sign

In alcuni casi di utilizzo, potrebbe essere necessario inviare un documento per le firme senza utilizzare un flusso di lavoro AEM. In questi casi, sarà molto comodo utilizzare i metodi di wrapper esposti dal bundle di esempio fornito in questo articolo.

## Distribuire il bundle OSGi di esempio

[Distribuire il bundle OSGi](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) tramite la console web OSGi dell’AEM. Specifica la chiave di integrazione API e l’utente API che utilizza la configurazione OSGi come mostrato di seguito, tramite Configuration Manager della console web OSGi dell’AEM.

 Tieni presente che `AdobeSignHelperMethods` Il bundle OSGi non è riconosciuto come codice di prodotto Adobe Experience Manager (AEM) e, come tale, non è supportato dal supporto Adobe.
![configurazione di accesso](assets/sign-configuration.png)


## Documentazione API

I seguenti elementi sono disponibili tramite `AcrobatSignHelperMethods` Servizio OSGi fornito nel bundle OSGi.

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


Documento utilizzato per creare un contratto o un modulo Web. Il documento viene caricato per la prima volta in Acrobat Sign dal mittente. Questo è noto come _transitorio_ poiché è disponibile per l’uso solo per 7 giorni dopo il caricamento. Questo metodo accetta `com.adobe.aemfd.docmanager.Document` e restituisce l&#39;ID documento transitorio.

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

Invia il documento per la firma utilizzando l’ID documento transitorio per la firma all’utente identificato dal parametro e-mail.

### getWidgetID

`String getWidgetID(String transientDocumentID)`

Un widget è come un modello riutilizzabile che può essere presentato agli utenti più volte e firmato più volte. Utilizza questo metodo per ottenere l&#39;ID del widget utilizzando l&#39;ID del documento transitorio.

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

Ottieni un URL widget per un ID widget specifico. Questo URL del widget può quindi essere presentato agli utenti per la firma del documento.

## Utilizzare l’API

Il `AcrobatSignHelperMethods` è un servizio OSGi, pertanto deve essere annotato utilizzando l’annotazione @Reference nel codice Java.

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```
