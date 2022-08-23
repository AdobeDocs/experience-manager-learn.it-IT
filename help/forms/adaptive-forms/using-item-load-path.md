---
title: Utilizzo del percorso di caricamento degli elementi per compilare l’elenco a discesa
description: Configura e compila un elenco a discesa per leggere i valori da un nodo crx
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
source-git-commit: 614db8b03a823b60846ab8ccfa8fbc29a41f7791
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 0%

---

# Proprietà di caricamento elemento in AEM Forms

Configura e compila l&#39;elenco a discesa utilizzando la proprietà del percorso di caricamento dell&#39;elemento.
Il campo Percorso di caricamento elemento consente all’autore di fornire un URL da cui caricare le opzioni disponibili in un elenco a discesa.
Per creare un tale nodo in crx, segui i passaggi indicati di seguito

* Accedi a crx
* Crea un nodo chiamato assets (puoi denominare questo nodo secondo le tue esigenze) digita sling:folder sotto content.
* Salva
* Fai clic sul nodo delle risorse appena create e impostane le relative proprietà come mostrato di seguito
* Sarà necessario creare una proprietà di tipo String denominata assettypes (puoi denominarla in base alle tue esigenze) con un valore multivalore. Immetti i valori desiderati e salva.

![item-load-path](assets/item-load-path-crx.png)

Per caricare questi valori nell’elenco a discesa, fornisci il seguente percorso nella proprietà del percorso di caricamento dell’elemento  **/content/assets/assettypes**

Il pacchetto di esempio può essere [scaricato da qui](assets/item-load-path-package.zip)
