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
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 2%

---

# Best practice  

I moduli di Adobe Experience Manager (AEM) consentono di trasformare transazioni complesse in esperienze digitali semplici e accattivanti. Il seguente documento descrive alcune best practice aggiuntive da seguire per lo sviluppo di Adaptive Forms. Questo documento deve essere utilizzato in combinazione con [presente documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenzioni di denominazione

* **Pannelli**
   * I nomi dei pannelli sono maiuscole in minuscole e viceversa.

* **Campi modulo**
   * I nomi dei campi sono maiuscole/minuscole che iniziano con un carattere minuscolo.
   * Non iniziare i nomi dei campi con numeri
   * Non includere trattini &quot;-&quot; nei tuoi nomi. Questo equivale a un segno meno nel codice e agisce come operatore nel codice.
   * I nomi possono contenere lettere, cifre, caratteri di sottolineatura e simboli di dollaro.
   * I nomi devono iniziare con una lettera
   * I nomi sono sensibili all’uso di maiuscole e minuscole
   * Le parole riservate (come le parole chiave JavaScript) non possono essere utilizzate come nomi. Fai attenzione ad altre parole riservate specifiche per AF, come &quot;pannello&quot;, &quot;nome&quot;.
   * Non includere trattini &quot;-&quot; nei tuoi nomi
* **Sviluppo di Forms**
   * Quando si sviluppano moduli di grandi dimensioni, è necessario tenere in considerazione i frammenti di modulo. Abilita il caricamento lento dei frammenti di modulo per tempi di caricamento più rapidi
   * **DataModel**
      * Si consiglia di associare il modulo adattivo al modello dati appropriato
   * **Eventi oggetto**
      * Il codice relativo alla visibilità di un oggetto deve sempre essere posizionato nell&#39;evento di visibilità di tale oggetto.
   * **Script**
      * Se il codice che si sta scrivendo all’interno di un modulo adattivo si estende oltre le 5 righe visibili, è necessario spostare il codice in una libreria client. Idealmente, aggiungi la tua funzione alla libreria client e quindi aggiungi i tag jsdoc appropriati per consentire alla funzione di essere visibile nell’editor di regole per i moduli adattivi.
