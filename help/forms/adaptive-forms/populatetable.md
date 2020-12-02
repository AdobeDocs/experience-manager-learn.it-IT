---
title: 'Compilazione tabella modulo adattivo '
seo-title: Compilazione tabella modulo adattivo
description: Compilare la tabella del modulo adattivo con i risultati delle richieste di servizi del modello dati modulo
seo-description: Compilare la tabella del modulo adattivo con i risultati delle richieste di servizi del modello dati modulo
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Compilazione di una tabella modulo adattiva con i risultati dell&#39;invito al servizio del modello dati modulo

[Il modulo Live è ospitato ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
quiIn questo articolo viene illustrato come compilare la tabella dei moduli adattivi recuperando i dati dalla chiamata al servizio del modello dati del modulo. Stiamo per creare un programma di ammortamento in una tabella che elenca ogni pagamento regolare su un mutuo nel tempo. I risultati dell&#39;ammortamento vengono restituiti dal modello dati modulo. Il servizio del modello dati modulo viene richiamato sull&#39;evento click del pulsante calculate, come mostrato nella schermata. I parametri di input e di output della chiamata del servizio sono mappati correttamente come mostrato nella schermata. L&#39;output è mappato alle colonne di Row1
![click event](assets/amortization.PNG)

La riga1 è configurata in modo che cresca a seconda dei dati restituiti dalla chiamata del servizio. Notate le impostazioni di ripetizione specificate qui. Il valore -1 indica un numero illimitato di righe nella tabella
![Riga1](assets/rowconfiguration.PNG)

## Distribuisci sul server

[Installa Tomcat come specificato ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[quiDistribuisci il ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[file SampleRest.warInstalla le risorse  ](assets/amortizationschedule.zip) utilizzando AEM gestore pacchetti 
[Apri il ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
modulo di pianificazione dell&#39;ammortamentoImmetti il valore appropriato e fai clic su Calcola programma di ammortamento.

