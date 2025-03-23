---
title: Sviluppo per differenza di pagina in AEM Sites
description: Questo video mostra come fornire stili personalizzati per la funzionalità Differenza pagina di AEM Sites.
feature: Authoring
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
duration: 281
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# Sviluppo per differenza di pagina {#developing-for-page-difference}

Questo video mostra come fornire stili personalizzati per la funzionalità Differenza pagina di AEM Sites.

## Personalizzazione degli stili di pagina diversi {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>Questo video aggiunge file CSS personalizzati alla libreria client we.Retail, dove le modifiche devono essere apportate al progetto AEM Sites del personalizzatore, nel codice di esempio seguente: `my-project`.

La differenza di pagina di AEM ottiene il CSS OOTB tramite un caricamento diretto di `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

A causa di questo carico diretto di CSS, anziché utilizzare una categoria di librerie client, è necessario trovare un altro punto di inserimento per gli stili personalizzati, e questo punto di inserimento personalizzato è la libreria client di authoring del progetto.

Questo ha il vantaggio di consentire che queste sostituzioni di stile personalizzate siano specifiche del tenant.

### Preparare la libreria client di authoring {#prepare-the-authoring-clientlib}

Verificare l&#39;esistenza di una clientlib `authoring` per il progetto in `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fornisci CSS personalizzato {#provide-the-custom-css}

Aggiungi alla libreria client `authoring` del progetto una `css.txt` che punti al file meno che fornirà gli stili di sostituzione. [Less](https://lesscss.org/) è preferito a causa delle sue molte funzioni convenienti, tra cui il wrapping delle classi, utilizzato in questo esempio.

```shell
base=./css

htmldiff.less
```

Creare il file `less` che contiene le sostituzioni di stile in `/apps/my-project/clientlibs/authoring/css/htmldiff.less` e fornire gli stili sovrascritti in base alle esigenze.

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

Includi la categoria clientlibs di authoring nella pagina base del progetto `/apps/my-project/components/structure/page/customheaderlibs.html` direttamente prima del tag `</head>` per garantire il caricamento degli stili.

Questi stili devono essere limitati alle modalità WCM [!UICONTROL Modifica] e [!UICONTROL anteprima].

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Il risultato finale di una pagina con differenze e gli stili sopra applicati sarà simile al seguente (HTML aggiunto e Componente modificato).

![Differenza di pagina](assets/page-diff.png)

## Risorse aggiuntive {#additional-resources}

* [Scarica il sito di esempio we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Utilizzo delle librerie client di AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Meno documentazione CSS](https://lesscss.org/)
