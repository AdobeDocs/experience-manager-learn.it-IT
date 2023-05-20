---
title: Personalizza componente Riepilogo
description: Estendi il componente della fase di riepilogo per includere la possibilità di passare al modulo successivo nel pacchetto.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# Personalizza passaggio di riepilogo

Il componente Passaggio di riepilogo viene utilizzato per visualizzare il riepilogo dell’invio del modulo con un collegamento per scaricare il modulo firmato. Il passaggio di riepilogo viene in genere inserito nell’ultimo pannello del modulo.
Ai fini di questo caso d’uso, abbiamo creato un nuovo componente basato sul componente predefinito Riepilogo e abbiamo esteso la funzionalità per includere la libreria client personalizzata.

Questo componente è identificato dall’etichetta Firma più modulo

La schermata seguente mostra il nuovo componente creato per visualizzare il messaggio al termine della cerimonia di firma

![componente di riepilogo](assets/summary.PNG)

Il nuovo componente è basato sul componente di riepilogo fornito con il prodotto.
![component-prop](assets/componentprop.PNG)

È stato aggiunto un pulsante per passare al modulo successivo per la firma
![template-code](assets/template-code.PNG)

Summary.jsp ha il seguente codice. Ha un riferimento alla libreria client identificata dall’ID categoria **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Risorse

Il componente di riepilogo personalizzato può essere [scaricato da qui](assets/custom-summary-step.zip)

## Passaggi successivi

[Ottieni il modulo successivo per la firma](./create-client-lib.md)