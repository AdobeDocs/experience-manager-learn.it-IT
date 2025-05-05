---
title: Personalizzazione delle icone dei componenti in Adobe Experience Manager Sites
description: Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze web in modo più rapido che mai.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Personalizzazione delle icone dei componenti {#developing-component-icons-in-aem-sites}

Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze web in modo più rapido che mai.

>[!VIDEO](https://video.tv.adobe.com/v/41681?quality=12&learn=on&captions=ita)

Il browser Componenti ora visualizza in un tema grigio coerente, con i seguenti elementi:

* **[!UICONTROL Gruppo di componenti]**
* **[!UICONTROL Titolo componente]**
* **[!UICONTROL Descrizione componente]**
* **[!UICONTROL Icona componente]**
   * Le prime due lettere del titolo del componente *(impostazione predefinita)*
   * Immagine PNG personalizzata *(configurata da uno sviluppatore)*
   * Immagine SVG personalizzata *(configurata da uno sviluppatore)*
   * Icona CoralUI *(configurata da uno sviluppatore)*

## Opzioni di configurazione per l’icona del componente {#component-icon-configuration-options}

### Abbreviazioni {#abbreviations}

Per impostazione predefinita, i primi 2 caratteri del titolo del componente (**[cq:Component]@jcr:title**) vengono utilizzati come abbreviazione. Ad esempio, se **[cq:Component]@jcr:title=Article List** l&#39;abbreviazione verrà visualizzata come &quot;**Ar**&quot;.

L&#39;abbreviazione può essere personalizzata tramite la proprietà **[cq:Component]@abbreviation**. Anche se questo valore può accettare più di 2 caratteri, si consiglia di limitare l’abbreviazione a 2 caratteri per evitare disturbi visivi.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icone di CoralUI {#coralui-icons}

Le icone di CoralUI fornite da AEM possono essere utilizzate per le icone dei componenti. Per configurare un&#39;icona di CoralUI, impostare una proprietà **[cq:Component]@cq:icon** sul valore dell&#39;attributo dell&#39;icona di CoralUI HTML desiderato (enumerato nella [documentazione di CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Immagini PNG {#png-images}

Le immagini PNG possono essere utilizzate per le icone dei componenti. Per configurare un&#39;immagine PNG come icona del componente, aggiungere l&#39;immagine desiderata come **nt:file** denominata **cq:icon.png** in **[cq:Component]**.

Il file PNG deve avere uno sfondo trasparente o un colore di sfondo impostato su **#707070**.

Le immagini PNG vengono ridimensionate a **20px di 20px**. Tuttavia, per adattarsi alle visualizzazioni retina **40px** di **40px** potrebbe essere preferibile.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Immagini SVG {#svg-images}

Le immagini SVG (basate su vettori) possono essere utilizzate per le icone dei componenti. Per configurare un&#39;immagine SVG come icona di un componente, aggiungere il SVG desiderato come **nt:file** denominato **cq:icon.svg** in **[cq:Component]**.

Le immagini SVG devono avere un colore di sfondo impostato su **#707070** e una dimensione di **20px per 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Risorse aggiuntive {#additional-resources}

* [Icone CoralUI disponibili](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
