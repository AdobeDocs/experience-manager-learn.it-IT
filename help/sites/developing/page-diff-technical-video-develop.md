---
title: Sviluppo per differenze di pagina in  AEM Sites
description: In questo video viene illustrato come fornire stili personalizzati per la funzionalità Differenza pagina di AEM Sites.
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 2%

---


# Sviluppo per differenza di pagina {#developing-for-page-difference}

In questo video viene illustrato come fornire stili personalizzati per la funzionalità Differenza pagina di AEM Sites.

## Personalizzazione degli stili delle differenze di pagina {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>In questo video viene aggiunto CSS personalizzato alla libreria client we.Retail, in cui durante le modifiche apportate al progetto AEM Sites  del cliente; nel codice di esempio seguente: `my-project`.

AEM differenza di pagina ottiene il CSS OOTB tramite un carico diretto di `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

A causa di questo carico diretto di CSS invece di utilizzare una categoria di libreria client, è necessario trovare un altro punto di inserimento per gli stili personalizzati, e questo punto di iniezione personalizzato è la clientlib di authoring del progetto.

Questo ha il vantaggio di consentire a queste sostituzioni di stile personalizzate di essere specifiche per il tenant.

### Preparare la clientlib di authoring {#prepare-the-authoring-clientlib}

Verificare l&#39;esistenza di una clientlib `authoring` per il progetto in `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fornire il CSS personalizzato {#provide-the-custom-css}

Aggiungete alla clientlib `authoring` del progetto un `css.txt` che punta al file minore che fornirà gli stili di sostituzione. [La ](https://lesscss.org/) lezione è preferita per le sue numerose e comode funzioni, tra cui il wrapping delle classi, che viene sfruttato in questo esempio.

```shell
base=./css

htmldiff.less
```

Create il file `less` che contiene le sostituzioni di stile in `/apps/my-project/clientlibs/authoring/css/htmldiff.less` e fornite gli stili di sostituzione come necessario.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### Includere il CSS clientlib di authoring tramite il componente di pagina {#include-the-authoring-clientlib-css-via-the-page-component}

Includete la categoria clientlibs di authoring nella pagina di base del progetto `/apps/my-project/components/structure/page/customheaderlibs.html` direttamente prima del tag `</head>` per garantire il caricamento degli stili.

Tali stili devono essere limitati alle modalità [!UICONTROL Edit] e [!UICONTROL preview] WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Il risultato finale di una pagina diversa con gli stili applicati sopra sarà simile a questo (HTML aggiunto e componente modificato).

![Differenza pagina](assets/page-diff.png)

## Risorse aggiuntive {#additional-resources}

* [Scaricate il sito di esempio we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Utilizzo AEM librerie client](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Meno documentazione CSS](https://lesscss.org/)
