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

[Il modulo Live è ospitato qui](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)In questo articolo viene illustrato come compilare la tabella dei moduli adattivi recuperando i dati dalla chiamata al servizio del modello dati del modulo. Stiamo per creare un programma di ammortamento in una tabella che elenca ogni pagamento regolare su un mutuo nel tempo. I risultati dell&#39;ammortamento vengono restituiti dal modello dati modulo. Il servizio del modello dati modulo viene richiamato sull&#39;evento click del pulsante calculate, come mostrato nella schermata. I parametri di input e di output della chiamata del servizio sono mappati correttamente come mostrato nella schermata. L&#39;output è mappato alle colonne di Row1![clickevent](assets/amortization.PNG)

La riga1 è configurata in modo che cresca a seconda dei dati restituiti dalla chiamata del servizio. Notate le impostazioni di ripetizione specificate qui. Il valore -1 indica un numero illimitato di righe nella tabella![Riga1](assets/rowconfiguration.PNG)

## Distribuisci sul server

[Installare Tomcat come specificato qui](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)[Distribuire il file](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)SampleRest.war[Installare le risorse ](assets/amortizationschedule.zip) utilizzando AEM gestoreAprire il modulo[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)Programma di ammortamentoImmettere il valore appropriato e fare clic su calculateProgramma di ammortamento deve essere compilato nel modulo

