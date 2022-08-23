---
title: Personalizzazione delle icone dei componenti in Adobe Experience Manager Sites
description: Le icone dei componenti consentono agli autori di identificare rapidamente un componente con icone o abbreviazioni significative. Gli autori possono ora individuare i componenti necessari per creare le proprie esperienze web in modo più rapido che mai.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 1%

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
   * Immagine PNG personalizzata *(configurato da uno sviluppatore)*
   * Immagine SVG personalizzata *(configurato da uno sviluppatore)*
   * Icona CoralUI *(configurato da uno sviluppatore)*

## Opzioni di configurazione dell’icona del componente {#component-icon-configuration-options}

### Abbreviazioni {#abbreviations}

Per impostazione predefinita, i primi 2 caratteri del titolo del componente (**[cq:Component]@jcr:title**) viene utilizzata come abbreviazione. Ad esempio, se **[cq:Component]@jcr:title=Elenco articoli** l&#39;abbreviazione viene visualizzata come &quot;**Air**&quot;.

L&#39;abbreviazione può essere personalizzata tramite **[cq:Component]Abbreviazione @** proprietà. Anche se questo valore può accettare più di 2 caratteri, si consiglia di limitare l’abbreviazione a 2 caratteri per evitare disturbi visivi.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Icone dell’interfaccia utente Coral {#coralui-icons}

Le icone CoralUI, fornite da AEM, possono essere utilizzate per le icone dei componenti. Per configurare un&#39;icona CoralUI, imposta un **[cq:Component]@cq:icon** al valore dell&#39;attributo dell&#39;icona HTML dell&#39;icona CoralUI desiderata (enumerato nel [Documentazione di CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Immagini PNG {#png-images}

Le immagini PNG possono essere utilizzate per le icone dei componenti. Per configurare un’immagine PNG come icona di un componente, aggiungi l’immagine desiderata come **nt:file** denominato **cq:icon.png** in **[cq:Component]**.

Il PNG deve avere uno sfondo trasparente o un colore di sfondo impostato su **#707070**.

Le immagini PNG verranno ridimensionate in **20 px x 20 px**. Tuttavia, per soddisfare le esigenze dei display Retina **40 px** da **40 px** potrebbe essere preferibile.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Immagini SVG {#svg-images}

Le immagini SVG (basate su vettori) possono essere utilizzate per le icone dei componenti. Per configurare un’immagine di SVG come icona di un componente, aggiungi il SVG desiderato come **nt:file** denominato **cq:icon.svg** in **[cq:Component]**.

Le immagini SVG devono avere un colore di sfondo impostato su **#707070** e le dimensioni **20px per 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Risorse aggiuntive {#additional-resources}

* [Icone CoralUI disponibili](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
