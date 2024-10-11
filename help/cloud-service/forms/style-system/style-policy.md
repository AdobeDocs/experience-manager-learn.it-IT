---
title: Utilizzo del sistema di stili in AEM Forms
description: Definire il criterio di stile per il componente pulsante
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
source-wordcount: '139'
ht-degree: 3%

---

# Definisci lo stile nel criterio per il componente

* Accedi all’istanza AEM locale predisposta per il cloud e passa a Strumenti | Generale | Modelli | il nome del progetto.

* Seleziona e apri il modello **Vuoto con componenti core** in modalità di modifica.
* Fai clic sull’icona del componente pulsante per aprire l’editor dei criteri.

* ![pulsante-criterio](assets/button-policy.png)

Definisci il criterio come mostrato di seguito

![dettagli-criteri-pulsante](assets/styling-policy.png)

Abbiamo definito 2 stili/varianti denominate Marketing e Corporate. Queste varianti sono associate alle classi CSS corrispondenti.**Assicurarsi che non ci sia spazio prima e dopo i nomi delle classi CSS**.
Salva le modifiche.

| Stile | Classe CSS |
|-----------|------------------------------------|
| Marketing | cmp-adaptiveform-button—marketing |
| Aziendale | cmp-adaptiveform-button - aziendale |

Queste classi CSS verranno definite nel file scss del componente (_button.scss).

## Passaggi successivi

[Definire le classi CSS](./create-variations.md)
