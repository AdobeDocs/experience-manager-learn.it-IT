---
title: Installazioni mobili headless AEM
description: Scopri le considerazioni sulla distribuzione per le distribuzioni mobili headless AEM.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
duration: 31
source-git-commit: 23ea95cfdf7e4c9fde4b53e9f68079b4d267ca20
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---

# Installazioni mobili headless AEM

Le implementazioni per dispositivi mobili headless AEM sono app mobile native per iOS, Android™, ecc. che consumano e interagiscono con i contenuti dell’AEM in modo headless.

Le distribuzioni per dispositivi mobili richiedono una configurazione minima, in quanto le connessioni HTTP alle API headless dell’AEM non vengono avviate nel contesto di un browser.

## Configurazioni di distribuzione

La seguente configurazione di distribuzione deve essere implementata per le distribuzioni di app per dispositivi mobili.

| L’app mobile si connette a → | Autore AEM | Pubblicazione AEM | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Condivisione delle risorse tra le origini (CORS) | ✘ | ✘ | ✘ |
| [Host AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Esempio di app per dispositivi mobili

Un Adobe fornisce app mobili iOS e Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="app iOS" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="app iOS">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="app iOS">app iOS</a></p>
                   <p class="is-size-6">Un’app iOS di esempio, scritta in SwiftUI, che utilizza contenuti delle API GraphQL headless dell’AEM.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="app Android™" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="app Android">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="app Android™">app Android™</a></p>
                   <p class="is-size-6">Un esempio di app Java™ Android™ che utilizza contenuti delle API GraphQL headless dell’AEM.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
