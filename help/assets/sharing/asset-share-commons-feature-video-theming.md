---
title: Introduzione alla tempistica in Asset Share Commons
description: Materiali per le conoscenze tecniche e funzionali di Assets Share Commons
version: 6.4, 6.5
topic: Content Management
feature: Asset Distribution
role: Developer
level: Intermediate
last-substantial-update: 2022-06-22T00:00:00Z
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 1%

---

# Introduzione alla tempistica in Asset Share Commons {#asset-share-commons-theme}

Breve introduzione ai temi in Asset Share Commons. Il video illustra come creare un nuovo tema con una combinazione di colori personalizzata.

>[!VIDEO](https://video.tv.adobe.com/v/20572?quality=12&learn=on)

In questo video viene creato un nuovo tema basato sul tema scuro Asset Share Commons. La combinazione di colori corrisponderà a un logo personalizzato per conferire al sito un aspetto coerente.

## Variabili del tema

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

Scarica [Tema libreria client personalizzata](assets/asc-theme-demo.zip)

## Risorse aggiuntive{#additional-resources}

* [Download delle versioni di Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [Download della versione di ACS AEM Commons 3.11.0+](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
