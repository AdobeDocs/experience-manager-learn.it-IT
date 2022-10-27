---
title: Popolare tabella di moduli adattivi
description: Compilare una tabella di moduli adattivi con i risultati delle vocazioni al servizio Modello dati modulo
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Compilare una tabella di moduli adattivi con i risultati dell’invocazione a Form Data Model Service

[Il modulo live è ospitato qui](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
In questo articolo esaminiamo la compilazione della tabella dei moduli adattivi recuperando i dati dalla chiamata al servizio del modello di dati del modulo. Stiamo per creare un programma di ammortamento in una tabella che elenca ogni pagamento regolare su un mutuo nel tempo. I risultati dell’ammortamento vengono restituiti dal modello dati modulo. Il servizio di Form Data Model viene richiamato sull&#39;evento click del pulsante calculate , come mostrato nella schermata . I parametri di input e output della chiamata del servizio vengono mappati in modo appropriato come mostrato nella schermata. L&#39;output è mappato alle colonne della riga1
![clickevent](assets/amortization.PNG)

La riga1 è configurata in modo che cresca a seconda dei dati restituiti dalla chiamata del servizio. Osserva le impostazioni di ripetizione specificate qui. Il valore -1 indica un numero illimitato di righe nella tabella
![Riga1](assets/rowconfiguration.PNG)

## Distribuisci sul server

[Installa Tomcat come specificato qui](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Distribuisci il file SampleRest.war contenuto in questo file zip nel tuo Tomcat](assets/sample-rest.zip)
[Installare le risorse ](assets/amortizationschedule.zip) utilizzo di AEM package manager
[Apri il modulo di pianificazione dell’ammortamento](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Immettere il valore appropriato e fare clic su Calcola programma di ammortamento per compilare il modulo
