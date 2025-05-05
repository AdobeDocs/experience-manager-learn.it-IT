---
title: Aggiunta di icone personalizzate
description: Aggiungere icone personalizzate alle schede verticali
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16418
exl-id: 20e44be0-5490-4414-9183-bb2d2a80bdf0
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Aggiunta di icone personalizzate

L’aggiunta di icone personalizzate alle schede può migliorare l’esperienza utente e l’impatto visivo in alcuni modi:

* Maggiore facilità d’uso: le icone possono comunicare rapidamente lo scopo di ogni scheda, semplificando la ricerca immediata da parte degli utenti. Suggerimenti visivi come le icone consentono agli utenti di navigare in modo più intuitivo.

* Gerarchia visiva e focus: le icone creano una separazione più distinta tra le schede, migliorando la gerarchia visiva. Questo può aiutare le schede importanti a distinguersi e a guidare efficacemente l’attenzione degli utenti.
Seguendo questo articolo, dovresti essere in grado di posizionare le icone come mostrato di seguito

![icone](assets/icons.png)

## Prerequisiti

Per seguire questo articolo, devi acquisire familiarità con Git, creare e distribuire un progetto AEM utilizzando Cloud Manager, configurare una pipeline front-end in AEM Cloud Manager e un po’ di CSS. Se non conosci questi argomenti, segui l&#39;articolo [Utilizzo di temi per assegnare uno stile ai componenti core](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/using-themes-in-core-components#rename-env-file-theme-folder).

## Aggiungere icone al tema

Apri il progetto tema in codice di Visual Studio o in un altro editor a tua scelta.
Aggiungi le icone desiderate alla cartella delle immagini.
Le icone contrassegnate in rosso sono le nuove icone aggiunte.
![icone nuove](assets/newicons.png)

## Crea mappa-icone per memorizzare le icone

Crea la mappa di icone nel file _variable.scss. La mappa SCSS $icon-map è una raccolta di coppie chiave-valore, in cui ogni chiave rappresenta un nome di icona (come casa, famiglia, ecc.) e ogni valore è il percorso del file di immagine associato a tale icona.

![variable-scss](assets/variable_scss.png)

```css
$icon-map: (
    home: "./resources/images/home.png",
    family: "./resources/images/icons8-family-80.png",
    pdf: "./resources/images/pdf.png",
    income: "./resources/images/income.png",
    assets: "./resources/images/assets.png",
    cars: "./resources/images/cars.png"
);
```

## Aggiungi mixin

Aggiungi il codice seguente a _mixin.scss

```css
@mixin add-icon-to-vertical-tab($image-url) {
  display: inline-flex;
  align-self: center;
  &::before {
    content: "";
    display:inline-block;
    background: url($image-url) left center / cover no-repeat;
    margin-right: 8px; /* Space between icon and text */
    height:40px;
    width:40px;
    vertical-align:middle;
    
  }
  
}
```

Il mixin add-icon-to-vertical-tab è progettato per aggiungere un&#39;icona personalizzata accanto al testo su una scheda verticale. Consente di includere facilmente un’immagine come icona nelle schede, posizionandola accanto al testo e formattandola per garantirne la coerenza e l’allineamento.

Raggruppamento del mixin: ecco cosa fa ogni parte del mixin:

Parametri:

* $image-url: URL dell&#39;icona o dell&#39;immagine che si desidera visualizzare accanto al testo della scheda. Il passaggio di questo parametro rende il mixin versatile, in quanto consente di aggiungere icone diverse a schede diverse, in base alle esigenze.

* Stili applicati:

   * display: inline-flex: questo rende l’elemento un contenitore flex, allineando orizzontalmente qualsiasi contenuto nidificato (come l’icona e il testo).
   * align-self: center: assicura che l’elemento sia centrato verticalmente all’interno del relativo contenitore.
   * Pseudo-elemento (::before):
   * content: &quot;&quot;: inizializza lo pseudo-elemento ::before, che viene utilizzato per visualizzare l’icona come immagine di sfondo.
   * display: inline-block: imposta lo pseudo-elemento su inline-block, consentendogli di comportarsi come un&#39;icona posizionata in linea con il testo.
   * background: url($image-url) left center / cover no-repeat;: aggiunge l’immagine di sfondo utilizzando l’URL fornito tramite $image-url. L&#39;icona è allineata a sinistra e centrata verticalmente.

## Aggiorna _verticaltabs.scss

Ai fini dell’articolo , ho creato una nuova classe css (cmp-verticaltabs—marketing) per visualizzare le icone delle schede. In questa nuova classe estendiamo l’elemento tab aggiungendo le icone. L&#39;elenco completo della classe css è il seguente

```css
.cmp-verticaltabs--marketing
{
  .cmp-verticaltabs
    {
      &__tab 
        {
          cursor:pointer;
            @each $name, $url in $icon-map {
            &[data-icon-name="#{$name}"]
              {
                  @include add-icon-to-vertical-tab($url);
              }
            }
        }
    }
}
```

## Modificare il componente schede verticali

Copiare il file verticaltabs.html da ```/apps/core/fd/components/form/verticaltabs/v1/verticaltabs/verticaltabs.html``` e incollarlo nel componente verticaltabs del progetto. Aggiungere la seguente riga ```data-icon-name="${tab.name}"``` al file copiato sotto il ruolo li come illustrato nell&#39;immagine seguente
![icona dati](assets/data-icons.png)
stiamo impostando un attributo di dati personalizzato denominato data-icon-name con il valore del nome della scheda.Se il nome della scheda corrisponde al nome di un&#39;immagine nella mappa delle icone, l&#39;immagine corrispondente è associata alla scheda.



## Verifica il codice

Distribuisci il componente verticaltabs aggiornato nell’istanza cloud.
Distribuisci il tema aggiornato utilizzando la pipeline front-end.
Create una variante di stile per i componenti linguetta verticale come mostrato di seguito
![stile-variante](assets/verticaltab-style-variation.png)
È stata creata una variante di stile denominata Marketing associata alla classe CSS _&#x200B;**cmp-verticaltabs—marketing**&#x200B;_.
Crea un modulo adattivo con un componente scheda verticale. Associa il componente Scheda verticale alla variante di stile Marketing.
Aggiungi un paio di schede alle tabulazioni verticali e denominale in modo che corrispondano alle immagini definite nella mappa delle icone, ad esempio home,family.
![home-icon](assets/tab-name.png)

Visualizzare l&#39;anteprima del modulo, le icone appropriate associate alla scheda
