---
title: Visualizzazione del codice QR nel modulo adattivo
description: Visualizzare il codice QR in un modulo adattivo
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
source-git-commit: e20d9f80cc7e1c6f5f6c81233d9a5178551e2fa2
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Componente codice QR di esempio

L’incorporazione di un codice QR in un modulo adattivo può notevolmente migliorare la comodità e l’efficienza per gli utenti di accedere a informazioni aggiuntive relative al modulo.

Il componente di esempio utilizza [QRCode.js](https://davidshimjs.github.io/qrcodejs/).

QRCode.js è una libreria JavaScript per la creazione di QRCode, supporta Cross-browser con HTML5 Canvas e tag di tabella nel DOM.

Il componente genera il codice QR in base al valore specificato nella proprietà di configurazione del componente.
![immagine](assets/qr-code-url.png)

Il seguente codice è stato utilizzato nel body.jsp del componente qr-code-generator.

L&#39;&quot;url&quot; è l&#39;url che deve essere incorporato nel codice qr. Questo URL è specificato nelle proprietà di configurazione del componente codice QR.

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



Il codice che segue utilizza il metodo makeCode della libreria QRCode.js nella libreria client del componente qr-code-generator. Il codice QR generato viene aggiunto al div identificato da id **&quot;qrcode&quot;**.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## Distribuire le risorse sul server locale

* [Scarica e installa il componente codice QR tramite Gestione pacchetti.](assets/qrcode.zip)
* [Scarica e installa il modulo adattivo di esempio tramite Gestione pacchetti.](assets/form-with-qr-code.zip)
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled). La sezione della guida del modulo include il codice QR.


