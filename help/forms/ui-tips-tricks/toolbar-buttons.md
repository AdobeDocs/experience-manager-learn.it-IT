---
title: Spaziare i pulsanti successivo e precedente della barra degli strumenti
description: Spaziare i pulsanti della barra degli strumenti
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 50
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---

# Spazio sul pulsante della barra degli strumenti

Quando si aggiungono i pulsanti Successivo e Precedente alla barra degli strumenti in AEM Forms, per impostazione predefinita i pulsanti vengono posizionati uno accanto all&#39;altro. È possibile premere il pulsante Avanti all&#39;estrema destra della barra degli strumenti mantenendo il pulsante Indietro/Prec a sinistra

![spaziatura barra degli strumenti](assets/toolbar-spacing.png)


## Personalizzare lo stile della barra degli strumenti

Il caso d’uso precedente può essere facilmente realizzato utilizzando l’editor di stili. Dopo aver aggiunto il pulsante Precedente/Successivo alla barra degli strumenti, accertatevi di aver selezionato il livello Stile dal menu Modifica. Dopo aver selezionato la modalità di stile, selezionare la barra degli strumenti per aprire il relativo foglio delle proprietà di stile. Espandi la sezione Dimension e posizione e accertati di visualizzare tutte le proprietà. Imposta le seguenti proprietà
* Dimension e posizione
   * Larghezza: 100%
   * Posizione: relativa

Salva le modifiche

## Personalizzare lo stile del pulsante Avanti

Selezionare il pulsante Avanti e assicurarsi di aprire la finestra delle proprietà di stile del pulsante successivo (non il testo del pulsante successivo). Imposta le seguenti proprietà
* Dimension e posizione
   * position: absolute Top 1px Right 1px
* Bordo
   * Raggio bordo: 4px(superiore,destro,inferiore,sinistro)

Salva le modifiche
