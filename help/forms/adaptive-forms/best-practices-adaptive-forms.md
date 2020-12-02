---
title: Convenzioni di denominazione e procedure ottimali da seguire per la creazione di moduli adattivi
seo-title: Convenzioni di denominazione e procedure ottimali da seguire per la creazione di moduli adattivi
description: Convenzioni di denominazione e procedure ottimali da seguire per la creazione di moduli adattivi
seo-description: Convenzioni di denominazione e procedure ottimali da seguire per la creazione di moduli adattivi
feature: adaptive-forms
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 5b05dbe45babfcfcfc81995d9d48bc9b26b9b6c8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 1%

---

# Best practice  

I moduli Adobe Experience Manager (AEM) consentono di trasformare transazioni complesse in esperienze digitali semplici e coinvolgenti. Il seguente documento descrive alcune best practice aggiuntive da seguire nello sviluppo di Forms adattivo. Questo documento deve essere utilizzato insieme a [questo documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenzioni di denominazione

* **Pannelli**
   * I nomi dei pannelli sono lettere maiuscole/minuscole che iniziano con il carattere maiuscolo.

* **Campi modulo**
   * I nomi dei campi sono lettere maiuscole/minuscole che iniziano con un carattere minuscolo.
   * Non iniziare i nomi dei campi con numeri
   * Non includete trattini &quot;-&quot; nei vostri nomi. Equivale a un segno meno nel codice e fungerà da operatori nel codice.
   * I nomi possono contenere lettere, cifre, caratteri di sottolineatura e simboli di dollaro.
   * I nomi devono iniziare con una lettera
   * I nomi sono sensibili alle maiuscole/minuscole
   * Le parole riservate (come le parole chiave JavaScript) non possono essere utilizzate come nomi. Attenzione alle altre parole riservate specifiche AF, ad esempio   come &quot;panel&quot;, &quot;name&quot;.
   * Non includete trattini &quot;-&quot; nei vostri nomi
* **Sviluppo di Forms**
   * Durante lo sviluppo di moduli di grandi dimensioni è necessario tenere in considerazione i frammenti di modulo. Caricamento lento dei frammenti di modulo per un caricamento più rapido   time
   * **DataModel**
      * È consigliabile associare il modulo adattivo al modello dati appropriato
   * **Eventi oggetto**
      * Il codice relativo alla visibilità di un oggetto deve sempre essere inserito nell&#39;evento di visibilità di tale oggetto.
   * **Script**
      * Se il codice scritto all&#39;interno di un modulo adattivo supera le 5 righe visibili, è necessario spostare il codice in una libreria client. È consigliabile aggiungere la funzione alla libreria client, quindi aggiungere i tag jsdoc appropriati per rendere la funzione visibile nell&#39;editor delle regole per i moduli adattivi.


