---
title: Creazione del componente indirizzo
description: Creazione di un nuovo componente core indirizzo in AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: 280c9a30-e017-4bc0-9027-096aac82c22c
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 2%

---

# Crea componente indirizzo

Accedi a CRXDE dell’istanza cloud locale di AEM Forms.

Creare una copia del nodo ``/apps/bankingapplication/components/adaptiveForm/button`` e rinominarlo in addressblock. Seleziona il nodo addressblock e impostane le proprietà come mostrato di seguito.

>[!NOTE]
>
> ``bankingapplication`` è l&#39;appId fornito durante la creazione del progetto Maven. Questo appId potrebbe essere diverso nel tuo ambiente. Potete fare una copia di qualsiasi componente, mi è capitato di fare una copia del componente pulsante


![blocco di indirizzi](assets/address-properties.png)

## proprietà nodo cq-template

Selezionare il nodo ``cq-template`` sotto il nodo ``addressblock`` e impostarne le proprietà come illustrato di seguito. Il tipo di campo è impostato su panel
![cq-template](assets/cq-template.png)

## Aggiungi nodi in cq-template

Aggiungi i seguenti nodi di tipo ``nt:unstructured`` in ``cq-template``

* indirizzo stradale
* città
* zip
* stato

Questi nodi rappresentano i campi del componente blocco di indirizzi. I campi streetaddress, city e zip saranno un campo di immissione testo e il campo state sarà un campo a discesa.

## Impostare le proprietà del nodo streetaddress

>[!NOTE]
>
> La **_bankingapplication_** nel percorso fa riferimento all&#39;appId del progetto Maven. Questo potrebbe essere diverso nell’ambiente

Selezionare il nodo ``streetaddress`` e impostarne le proprietà come illustrato di seguito.
![indirizzo](assets/streetaddress.png)

## Imposta le proprietà del nodo della città

Selezionare il nodo ``city`` e impostarne le proprietà come illustrato di seguito.
![città](assets/city.png)

## Imposta le proprietà del nodo zip

Selezionare il nodo ``zip`` e impostarne le proprietà come illustrato di seguito.
![zip](assets/zip.png)

## Impostare le proprietà del nodo di stato

Selezionare il nodo ``state`` e impostarne le proprietà come illustrato di seguito. Osserva il fieldType dello stato: è impostato per essere un menu a discesa
![stato](assets/state.png)

## Impostare le opzioni per il campo Stato

Selezionare il nodo ``state`` e aggiungere le seguenti proprietà.

| Nome | Tipo | Valore |
|----------|----------|---------------------|
| enum | Stringa[] | CA, NY |
| enumNames | Stringa[] | California, New York |


Il componente addressblock finale si presenterà così

![indirizzo-finale](assets/crx-address-block.png)

## Passaggi successivi

[Distribuire il progetto](./deploy-your-project.md)
