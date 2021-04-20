---
title: Personalizzazione delle icone dei componenti in Adobe Experience Manager Sites
description: Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze web in modo più rapido che mai.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: Core Components
topic: Development
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 2%

---


# Personalizzazione delle icone dei componenti {#developing-component-icons-in-aem-sites}

Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze web in modo più rapido che mai.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

Il browser Componenti ora viene visualizzato in un tema grigio coerente e presenta i seguenti elementi:

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

Per impostazione predefinita, i primi 2 caratteri del titolo del componente (**[cq:Component]@jcr:title**) sono utilizzati come abbreviazione. Ad esempio, se **[cq:Component]@jcr:title=Article List** l’abbreviazione viene visualizzata come &quot;**Ar**&quot;.

L&#39;abbreviazione può essere personalizzata tramite la proprietà **[cq:Component]@abbreviation** . Anche se questo valore può accettare più di 2 caratteri, si consiglia di limitare l’abbreviazione a 2 caratteri per evitare disturbi visivi.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icone CoralUI {#coralui-icons}

Le icone CoralUI fornite da AEM possono essere utilizzate per le icone dei componenti. Per configurare un&#39;icona CoralUI, imposta una proprietà **[cq:Component]@cq:icon** sul valore dell&#39;attributo dell&#39;icona HTML dell&#39;icona CoralUI desiderata (enumerata nella [documentazione CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Immagini PNG {#png-images}

Le immagini PNG possono essere utilizzate per le icone dei componenti. Per configurare un&#39;immagine PNG come icona di un componente, aggiungi l&#39;immagine desiderata come **nt:file** denominato **cq:icon.png** sotto **[cq:Component]**.

Il PNG deve avere uno sfondo trasparente o un colore di sfondo impostato su **#707070**.

Le immagini PNG verranno ridimensionate a **20px per 20px**. È tuttavia preferibile utilizzare display Retina **40px** di **40px**.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Immagini SVG {#svg-images}

Le immagini SVG (vettoriali) possono essere utilizzate per le icone dei componenti. Per configurare un&#39;immagine SVG come icona di un componente, aggiungi l&#39;SVG desiderato come **nt:file** denominato **cq:icon.svg** sotto **[cq:Component]**.

Le immagini SVG devono avere un colore di sfondo impostato su **#707070** e una dimensione di **20px per 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Risorse aggiuntive {#additional-resources}

* [Icone CoralUI disponibili](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
