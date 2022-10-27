---
title: Spaziare i pulsanti successivo e precedente della barra degli strumenti
description: Spazio dei pulsanti della barra degli strumenti
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Spazio del pulsante della barra degli strumenti

Quando si aggiungono i pulsanti Successivo e Precedente alla barra degli strumenti in AEM Forms, per impostazione predefinita i pulsanti sono posizionati uno accanto all’altro. È possibile premere il pulsante Avanti all&#39;estrema destra nella barra degli strumenti mantenendo il pulsante precedente/indietro a sinistra

![spaziatura barra degli strumenti](assets/toolbar-spacing.png)


## Personalizzare lo stile della barra degli strumenti

Il caso d’uso di cui sopra può essere facilmente realizzato utilizzando l’editor di stili. Dopo aver aggiunto il pulsante Prec/Succ alla barra degli strumenti, accertati di aver selezionato il livello Stile dal menu di modifica. Con la modalità stile selezionata, selezionare la barra degli strumenti per aprire il relativo foglio delle proprietà di stile. Espandi la sezione Dimension e posizione e assicurati di visualizzare tutte le proprietà. Imposta le seguenti proprietà
* Dimension e posizione
   * Larghezza: 100%
   * Posizione: relativo

Salva le modifiche

## Personalizzare lo stile del pulsante Avanti

Selezionare il pulsante Avanti e assicurarsi di aprire il foglio delle proprietà stile del pulsante successivo (non il testo del pulsante successivo). Imposta le seguenti proprietà
* Dimension e posizione
   * posizione: assoluto superiore 1px destra 1px
* Bordo
   * Raggio bordo: 4px(In alto, A destra, In basso, A sinistra)

Salva le modifiche
