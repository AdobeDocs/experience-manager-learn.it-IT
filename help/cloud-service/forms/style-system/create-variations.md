---
title: Utilizzo del sistema di stili in AEM Forms
description: Definire le classi CSS per le varianti
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Creare varianti per il componente pulsante

Dopo aver clonato il tema, apri il progetto utilizzando il codice di Visual Studio. Dovresti vedere una visualizzazione simile
nel codice di visual studio
![project explorer](assets/easel-theme.png)

Aprire il file src->components->button->_button.scss. Definiremo le varianti personalizzate in questo file.

## Variante aziendale

```css
.cmp-adaptiveform-button-corporate {
  @include container;
  .cmp-adaptiveform-button {
    &__widget {
      @include primary-button;
      background: $brand-red;
      text-transform: uppercase;
      border-radius: 0px;
      color: yellow;
    }
  }
}
```

## Spiegazione

* **cmp-adaptiveform-button—corporate**: classe wrapper o container principale per il componente &quot;cmp-adaptiveform-button—corporate&quot;.
Qualsiasi stile o mixin all’interno di questo blocco verrà applicato agli elementi all’interno di questa classe.
* **@include contenitore**: utilizza un mixin denominato contenitore, definito in _mixins.scss. Il contenitore mixin applica in genere stili relativi al layout, ad esempio l’impostazione di margini, spaziature o altri stili strutturali per garantire che il contenitore si comporti in modo coerente.
* **.cmp-adaptiveform-button**: All&#39;interno del blocco corporate-style-button, l&#39;elemento figlio viene indirizzato con la classe .cmp-adaptiveform-button.
* **&amp;__widget**: il simbolo &amp; fa riferimento al selettore principale, che in questo caso è .cmp-adaptiveform-button.
Ciò significa che la classe finale di destinazione sarà .cmp-adaptiveform-button__widget, una classe in stile BEM (Block Element Modifier) che rappresenta un sottocomponente (l’elemento __widget) all’interno del blocco .cmp-adaptiveform-button.
* **@include pulsante primario**: include un mixin primario, definito nel file _mixin.scss, che aggiunge stili correlati al pulsante, ad esempio spaziatura interna, colori, effetti al passaggio del mouse e così via. Le proprietà background,text-transform,border-radius,color definite nel mixin primary-button vengono ignorate.

Il file _mixins.scss è definito in src->site come mostrato nella schermata seguente

![mixin.scss](assets/mixins.png)

## Variante di marketing

```css
.cmp-adaptiveform-button--marketing {
  
  @include container;
  .cmp-adaptiveform-button {
  &__widget {
    @include primary-button;
    background-color: #3498db;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 50px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: all 0.3s ease;
    outline: none;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    &:hover:not([disabled]) {
      position: relative;
      scale: 102%;
      transition: box-shadow 0.1s ease-out, transform 0.1s ease-out;
      background-color: #2980b9;
      box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
      transform: translateY(-3px);
    }
  }
}
  
}
```

## Passaggi successivi

[Testare le varianti](./build.md)


