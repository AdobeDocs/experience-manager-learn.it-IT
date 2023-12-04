---
title: Convenzioni di denominazione e best practice da seguire per la creazione di moduli adattivi
description: Convenzioni di denominazione e best practice da seguire per la creazione di moduli adattivi
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 83
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# Best practice

I moduli di Adobe Experience Manager (AEM) possono aiutarti a trasformare transazioni complesse in esperienze digitali semplici e deliziose. Il documento seguente descrive alcune best practice aggiuntive da seguire durante lo sviluppo di Adaptive Forms. Questo documento deve essere utilizzato insieme a [questo documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenzioni di denominazione

* **Pannelli**
   * I nomi dei pannelli iniziano con una lettera maiuscola e iniziano con una maiuscola.

* **Campi modulo**
   * I nomi dei campi sono composti da lettere maiuscole e minuscole e iniziano con un carattere minuscolo.
   * Non iniziare i nomi dei campi con numeri
   * Non includere trattini &quot;-&quot; nel nome. Queste corrisponderanno a un segno meno nel codice e agiranno come operatori nel codice.
   * I nomi possono contenere lettere, cifre, trattini bassi e simboli di dollaro.
   * I nomi devono iniziare con una lettera
   * I nomi distinguono tra maiuscole e minuscole
   * Le parole riservate (come le parole chiave JavaScript) non possono essere utilizzate come nomi. Fai attenzione ad altre parole riservate specifiche per AF come &quot;panel&quot;, &quot;name&quot;.
   * Non includere trattini &quot;-&quot; nei nomi
* **Sviluppo di Forms**
   * I frammenti di modulo devono essere considerati durante lo sviluppo di moduli di grandi dimensioni. Attiva il caricamento lento dei frammenti di modulo per tempi di caricamento più rapidi
   * **DataModel**
      * Si consiglia di associare il modulo adattivo al modello dati appropriato

   * **Eventi oggetto**
      * Il codice relativo alla visibilità di un oggetto deve sempre essere posizionato nell&#39;evento di visibilità di tale oggetto.
   * **Script**
      * Se il codice che si sta scrivendo all’interno di un modulo adattivo si estende oltre le 5 righe visibili, è necessario spostare il codice in una libreria client. È consigliabile aggiungere la funzione alla libreria client e quindi aggiungere i tag jsdoc appropriati per consentire alla funzione di essere visibile nell’editor di regole per moduli adattivi.
