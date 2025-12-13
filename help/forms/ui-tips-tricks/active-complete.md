---
title: Personalizzare lo stile delle schede di navigazione a sinistra con icone
description: Aggiungere icone per indicare le schede attive e completate
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
duration: 68
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 21%

---

# Aggiungere icone per indicare le schede attive e completate

Se disponi di un modulo adattivo con navigazione a sinistra nella scheda, potrebbe essere utile visualizzare le icone per indicare lo stato della scheda. Ad esempio, vuoi mostrare un’icona per indicare la scheda attiva e l’icona per indicare la scheda completata come mostrato nella schermata seguente.

![spaziatura barra degli strumenti](assets/active-completed.png)

## Creare un modulo adattivo

Per creare il modulo di esempio è stato utilizzato un semplice modulo adattivo basato sul modello Base e sul tema Canvas 3.0.
Le [icone utilizzate in questo articolo](assets/icons.zip) possono essere scaricate da qui.


## Personalizzare lo stile dello stato predefinito

Apri il modulo in modalità di modifica
Accertatevi di trovarvi nel livello di stile e selezionate una scheda qualsiasi (ad esempio la scheda Generale).
Lo stato predefinito si trova quando apri l’editor di stili per la scheda, come illustrato nella schermata seguente
![scheda di navigazione](assets/navigation-tab.png)

Imposta le proprietà CSS per lo stato predefinito come mostrato di seguito

| Categoria | Nome proprietà | Valore proprietà |
|:---|:---|:---|
| Dimensioni e posizione | Larghezza | 50 px |
| Testo | Spessore carattere | Grassetto |
| Testo | Colore | #FFF |
| Testo | Altezza riga | 3 |
| Testo | Allineamento testo | Sinistra |
| Esperienza pregressa | Colore | #056dae |

Salva le modifiche

## Personalizzare lo stile dello stato attivo

Assicurati di essere nello stato Attivo e di applicare lo stile alle seguenti proprietà CSS

| Categoria | Nome proprietà | Valore proprietà |
|:---|:---|:---|
| Dimensioni e posizione | Larghezza | 50 px |
| Testo | Spessore carattere | Grassetto |
| Testo | Colore | #FFF |
| Testo | Altezza riga | 3 |
| Testo | Allineamento testo | Sinistra |
| Esperienza pregressa | Colore | #056dae |

Personalizzare lo stile dell&#39;immagine di sfondo come mostrato nella schermata seguente

Salva le modifiche.



![stato-attivo](assets/active-state.png)

## Personalizzare lo stile dello stato visitato

Accertati di essere nello stato visitato e assegna uno stile alle seguenti proprietà

| Categoria | Nome proprietà | Valore proprietà |
|:---|:---|:---|
| Dimensioni e posizione | Larghezza | 50 px |
| Testo | Spessore carattere | Grassetto |
| Testo | Colore | #FFF |
| Testo | Altezza riga | 3 |
| Testo | Allineamento testo | Sinistra |
| Esperienza pregressa | Colore | #056dae |

Personalizzare lo stile dell&#39;immagine di sfondo come mostrato nella schermata seguente


![stato visitato](assets/visited-state.png)

Salva le modifiche

Visualizzare l&#39;anteprima del modulo e verificare che le icone funzionino come previsto.
