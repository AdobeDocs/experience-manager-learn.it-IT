---
title: Sviluppo per differenze di pagina in AEM Sites
description: Questo video mostra come fornire stili personalizzati per la funzionalità Differenza pagina di AEM Sites.
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 4%

---

# Sviluppo per differenza di pagina {#developing-for-page-difference}

Questo video mostra come fornire stili personalizzati per la funzionalità Differenza pagina di AEM Sites.

## Personalizzazione degli stili di differenza della pagina {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>Questo video aggiunge CSS personalizzati alla libreria client we.Retail, in cui, come tali modifiche devono essere apportate al progetto AEM Sites del cliente; nel codice di esempio seguente: `my-project`.

AEM differenza di pagina ottiene il CSS OOTB tramite un carico diretto di `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

A causa di questo carico diretto di CSS anziché utilizzare una categoria di libreria client, dobbiamo trovare un altro punto di inserimento per gli stili personalizzati, e questo punto di iniezione personalizzato è la clientlib di authoring del progetto.

Questo ha il vantaggio di consentire a queste sostituzioni di stile personalizzate di essere specifiche del tenant.

### Preparare la clientlib dell’authoring {#prepare-the-authoring-clientlib}

Assicurare l&#39;esistenza di un `authoring` clientlib per il progetto all’indirizzo `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fornire i CSS personalizzati {#provide-the-custom-css}

Aggiungi al `authoring` clientlib a `css.txt` che punta al file less che fornirà gli stili di override. [Meno](https://lesscss.org/) è preferito a causa delle sue molte funzioni convenienti, tra cui il wrapping di classe che viene sfruttato in questo esempio.

```shell
base=./css

htmldiff.less
```

Crea il `less` file contenente le sostituzioni dello stile in `/apps/my-project/clientlibs/authoring/css/htmldiff.less`e fornisci gli stili di overring in base alle esigenze.

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

### Includere il CSS clientlib di authoring tramite il componente pagina {#include-the-authoring-clientlib-css-via-the-page-component}

Includi la categoria clientlibs di authoring nella pagina di base del progetto `/apps/my-project/components/structure/page/customheaderlibs.html` direttamente prima del `</head>` per garantire che gli stili vengano caricati.

Questi stili devono essere limitati a [!UICONTROL Modifica] e [!UICONTROL anteprima] Modalità WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Il risultato finale di una pagina con differenze con gli stili applicati sopra sarà simile a questo (HTML aggiunto e Componente modificato).

![Differenza tra pagine](assets/page-diff.png)

## Risorse aggiuntive {#additional-resources}

* [Scarica il sito di esempio di we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Utilizzo delle librerie client AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=it)
* [Meno documentazione CSS](https://lesscss.org/)
