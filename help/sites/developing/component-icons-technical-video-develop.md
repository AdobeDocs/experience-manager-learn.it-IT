---
title: Personalizzazione delle icone dei componenti in  Adobe Experience Manager Sites
description: Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze Web più rapidamente che mai.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 1%

---


# Personalizzazione delle icone dei componenti {#developing-component-icons-in-aem-sites}

Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze Web più rapidamente che mai.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

Il browser Componenti ora viene visualizzato in un tema grigio coerente, con i seguenti elementi:

* **[!UICONTROL Gruppo componente]**
* **[!UICONTROL Titolo componente]**
* **[!UICONTROL Descrizione componente]**
* **[!UICONTROL Icona componente]**
   * Le prime due lettere del titolo del componente *(predefinito)*
   * Immagine PNG personalizzata *(configurata da uno sviluppatore)*
   * Immagine SVG personalizzata *(configurata da uno sviluppatore)*
   * Icona CoralUI *(configurata da uno sviluppatore)*

## Opzioni di configurazione dell&#39;icona del componente {#component-icon-configuration-options}

### Abbreviazioni {#abbreviations}

Per impostazione predefinita, i primi 2 caratteri del titolo del componente (**[cq:Component]@jcr:title**) sono utilizzati come abbreviazione. Ad esempio, se **[cq:Component]@jcr:title=Article List** l&#39;abbreviazione sarà visualizzata come &quot;**Ar**&quot;.

L&#39;abbreviazione può essere personalizzata tramite la proprietà **[cq:Component]@abbreviation**. Anche se questo valore può accettare più di 2 caratteri, si consiglia di limitare l&#39;abbreviazione a 2 caratteri per evitare qualsiasi disturbo visivo.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icone CoralUI {#coralui-icons}

Le icone CoralUI, fornite da AEM, possono essere utilizzate per le icone dei componenti. Per configurare un&#39;icona CoralUI, impostare una proprietà **[cq:Component]@cq:icon** sul valore dell&#39;attributo dell&#39;icona dell&#39;icona CoralUI desiderato (enumerato nella [documentazione di CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Immagini PNG {#png-images}

Le immagini PNG possono essere usate per le icone dei componenti. Per configurare un&#39;immagine PNG come icona di un componente, aggiungete l&#39;immagine desiderata come **nt:file** denominato **cq:icon.png** in **[cq:Component]**.

Il file PNG deve avere uno sfondo trasparente o un colore di sfondo impostato su **#707070**.

Le immagini PNG verranno ridimensionate a **20px per 20px**. Tuttavia, per contenere display retina **40px** di **40px** potrebbe essere preferibile.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Immagini SVG {#svg-images}

Le immagini SVG (basate su vettore) possono essere usate per le icone dei componenti. Per configurare un&#39;immagine SVG come icona di un componente, aggiungete il file SVG desiderato come **nt:file** denominato **cq:icon.svg** sotto la **[cq:Component]**.

Le immagini SVG devono avere un colore di sfondo impostato su **#707070** e una dimensione di **20px per 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Risorse aggiuntive {#additional-resources}

* [Icone CoralUI disponibili](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
