---
title: Personalizza componente di riepilogo
description: Estendi il componente del passaggio di riepilogo per includere la possibilità di passare al modulo successivo nel pacchetto.
feature: Moduli adattivi
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

---


# Personalizza passaggio di riepilogo

Il componente Passaggio di riepilogo viene utilizzato per visualizzare il riepilogo dell’invio del modulo con un collegamento per scaricare il modulo firmato. Il passaggio di riepilogo viene in genere posizionato nell’ultimo pannello del modulo.
Ai fini di questo caso d’uso abbiamo creato un nuovo componente basato sul componente Riepilogo preconfigurato e abbiamo esteso la funzionalità per includere clientlib personalizzate.

Questo componente è identificato dall’etichetta Firma più moduli

La schermata seguente mostra il nuovo componente creato per visualizzare il messaggio al termine della cerimonia di firma

![componente di riepilogo](assets/summary.PNG)

Il nuovo componente si basa sul componente di riepilogo predefinito.
![component-prop](assets/componentprop.PNG)

È stato aggiunto un pulsante per passare al modulo successivo per la firma
![template-code](assets/template-code.PNG)

Il summary.jsp ha il seguente codice. Fa riferimento alla libreria client identificata dalla categoria id **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

Il componente di riepilogo personalizzato può essere [scaricato da qui](assets/custom-summary-step.zip)


