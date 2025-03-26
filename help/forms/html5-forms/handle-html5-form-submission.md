---
title: Gestire l’invio di un modulo HTML5
description: Crea gestore invio HTML5 Form.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# Gestire l’invio di moduli HTML5

I moduli HTML5 possono essere inviati a un servlet ospitato in AEM. I dati inviati sono accessibili nel servlet come flusso di input. Per inviare il modulo HTML5, aggiungi un &quot;pulsante HTTP di invio&quot; nel modello di modulo utilizzando AEM Forms Designer.

## Creare il gestore di invio

Un semplice servlet può gestire l’invio del modulo HTML5. Estrai i dati inviati utilizzando il seguente frammento di codice. Scarica il [servlet](assets/html5-submit-handler.zip) fornito in questa esercitazione. Installa il [servlet](assets/html5-submit-handler.zip) utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp).

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

Verificare di aver configurato la [configurazione SDK client Adobe LiveCycle](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) se si intende utilizzare il codice per richiamare un processo J2EE.

## Configurare l’URL di invio del modulo HTML5

![Invia URL](assets/submit-url.PNG)

- Apri l&#39;XDP e passa a _Proprietà_->_Avanzate_.
- Copiare http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html e incollarlo nel campo di testo URL di invio.
- Fare clic sul pulsante _SalvaEChiudi_.

### Aggiungi voce in Exclude Paths (Percorsi di esclusione)

- Vai a [configMgr](http://localhost:4502/system/console/configMgr).
- Cerca _Filtro CSRF Adobe Granite_.
- Aggiungere la voce seguente nella sezione Percorsi esclusi: _/content/AemFormsSamples/handlehml5formsubmit_.
- Salva le modifiche.

### Testare il modulo

- Apri il modello xdp.
- Fare clic su _Anteprima_->Anteprima come HTML.
- Immettere i dati nel modulo e fare clic su Invia.
- Controllare il file stdout.log del server per i dati inviati.

### Letture aggiuntive

Per ulteriori informazioni sulla generazione di PDF da invii di moduli HTML5, consulta questo [articolo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html).

