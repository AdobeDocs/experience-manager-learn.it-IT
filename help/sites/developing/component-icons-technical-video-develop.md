---
title: Personalizzazione delle icone dei componenti in Adobe Experience Manager Sites
description: Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze web in modo più rapido che mai.
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 389
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Personalizzazione delle icone dei componenti {#developing-component-icons-in-aem-sites}

Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze web in modo più rapido che mai.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

Il browser Componenti ora visualizza in un tema grigio coerente, con i seguenti elementi:

* **[!UICONTROL Gruppo di componenti]**
* **[!UICONTROL Titolo componente]**
* **[!UICONTROL Descrizione componente]**
* **[!UICONTROL Icona componente]**
   * Le prime due lettere del titolo del componente *(impostazione predefinita)*
   * Immagine PNG personalizzata *(configurato da uno sviluppatore)*
   * Immagine SVG personalizzata *(configurato da uno sviluppatore)*
   * Icona CoralUI *(configurato da uno sviluppatore)*

## Opzioni di configurazione per l’icona del componente {#component-icon-configuration-options}

### Abbreviazioni {#abbreviations}

Per impostazione predefinita, i primi 2 caratteri del titolo del componente (**[cq:Component]@jcr:titolo**) vengono utilizzati come abbreviazione. Ad esempio, se **[cq:Component]@jcr:title=Elenco articoli** l’abbreviazione viene visualizzata come &quot;**Ar**&quot;.

L’abbreviazione può essere personalizzata tramite il **[cq:Component]@abbreviation** proprietà. Anche se questo valore può accettare più di 2 caratteri, si consiglia di limitare l’abbreviazione a 2 caratteri per evitare disturbi visivi.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icone di CoralUI {#coralui-icons}

Le icone di CoralUI fornite dall’AEM possono essere utilizzate per le icone dei componenti. Per configurare un&#39;icona di CoralUI, imposta un **[cq:Component]@cq:icona** all&#39;icona HTML dell&#39;icona CoralUI desiderata (enumerata nella [Documentazione di CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Immagini PNG {#png-images}

Le immagini PNG possono essere utilizzate per le icone dei componenti. Per configurare un’immagine PNG come icona del componente, aggiungi l’immagine desiderata come icona **nt:file** denominato **cq:icon.png** sotto **[cq:Component]**.

Il PNG deve avere uno sfondo trasparente o un colore di sfondo impostato su **#707070**.

Le immagini PNG vengono ridimensionate in **20 px per 20 px**. Tuttavia, per adattarsi a display retinici **40 px** da **40 px** potrebbe essere preferibile.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Immagini SVG {#svg-images}

Le immagini SVG (basate su vettori) possono essere utilizzate per le icone dei componenti. Per configurare un’immagine SVG come icona di un componente, aggiungi il SVG desiderato come icona **nt:file** denominato **cq:icon.svg** sotto **[cq:Component]**.

Le immagini SVG devono avere un colore di sfondo impostato su **#707070** e una dimensione di **20 px per 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Risorse aggiuntive {#additional-resources}

* [Icone disponibili di CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
