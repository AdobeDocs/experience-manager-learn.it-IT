---
title: Utilizzo del percorso di caricamento elementi per compilare l’elenco a discesa
description: Configurare e popolare un elenco a discesa per leggere i valori da un nodo crx
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
duration: 51
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Proprietà Caricamento elemento in AEM Forms

Configurare e popolare l’elenco a discesa utilizzando la proprietà item load path.
Il campo Percorso di caricamento elemento consente all’autore di fornire un URL da cui caricare le opzioni disponibili in un elenco a discesa.
Per creare un nodo di questo tipo in crx, segui i passaggi indicati di seguito:
* Accedi a crx
* Crea un nodo denominato assets (puoi denominare questo nodo in base alle tue esigenze) e digita sling:folder in content.
* Salva
* Fai clic sul nodo delle nuove risorse creato e impostane le proprietà come mostrato di seguito
* Sarà necessario creare una proprietà di tipo String denominata assettypes(è possibile denominarla in base alle proprie esigenze).Assicurarsi che la proprietà sia multivalore. Immetti i valori desiderati e salvali.
  ![item-load-path](assets/item-load-path-crx.png)

Per caricare questi valori nell’elenco a discesa, specifica il seguente percorso nella proprietà percorso di caricamento elemento  **/content/assets/assettypes**

Il pacchetto di esempio può essere [scaricato da qui](assets/item-load-path-package.zip)
