---
title: Personalizza componente Riepilogo
description: Estende il componente passo di riepilogo per includere la possibilità di passare al modulo successivo nel pacchetto.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Personalizza passaggio di riepilogo

Il componente Passo di riepilogo viene utilizzato per visualizzare il riepilogo dell’invio del modulo con un collegamento per scaricare il modulo firmato. Il passaggio di riepilogo viene in genere posizionato nell&#39;ultimo pannello del modulo.
Ai fini di questo caso d’uso, abbiamo creato un nuovo componente basato sul componente Riepilogo out-of-the-box ed esteso la capacità di includere clientlib personalizzate.

Questo componente è identificato dall’etichetta Firma più moduli

La schermata seguente mostra il nuovo componente creato per visualizzare il messaggio al termine della cerimonia di firma

![componente di riepilogo](assets/summary.PNG)

Il nuovo componente è basato sul componente di riepilogo out-of-the-box.
![component-prop](assets/componentprop.PNG)

È stato aggiunto un pulsante per passare al modulo successivo per la firma
![template-code](assets/template-code.PNG)

Il file summary.jsp ha il seguente codice: Contiene un riferimento alla libreria client identificata dalla categoria id **getnextform**

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


