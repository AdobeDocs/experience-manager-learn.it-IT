---
title: Popolare tabella modulo adattivo
description: Popolare la tabella del modulo adattivo con i risultati delle chiamate al servizio del modello dati modulo
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 64
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Popolare la tabella del modulo adattivo con i risultati della chiamata al servizio del modello di dati del modulo

[Il modulo live è ospitato qui](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
In questo articolo esaminiamo come popolare la tabella dei moduli adattivi recuperando i dati dalla chiamata al servizio del modello di dati del modulo. Verrà creato un piano di ammortamento in una tabella che elenca ogni pagamento regolare di un mutuo nel tempo. I risultati dell’ammortamento vengono restituiti dal modello dati del modulo. Il servizio del modello dati modulo viene richiamato sull’evento di clic del pulsante calcola come mostrato nella schermata. I parametri di input e output della chiamata al servizio sono mappati in modo appropriato, come mostrato nella schermata. L&#39;output viene mappato alle colonne di Row1
![clickevent](assets/amortization.PNG)

Row1 è configurato per l’aumento di dimensione a seconda dei dati restituiti dalla chiamata del servizio. Osserva le impostazioni di ripetizione specificate qui. Il valore -1 indica un numero illimitato di righe nella tabella
![Riga 1](assets/rowconfiguration.PNG)

## Distribuisci sul server

[Installa Tomcat come specificato qui](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Distribuisci il file SampleRest.war contenuto in questo file zip nel tuo Tomcat](assets/sample-rest.zip)
[Installare le risorse](assets/amortizationschedule.zip) utilizzo del gestore di pacchetti AEM
[Aprire il modulo Programma di ammortamento](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Inserire il valore appropriato e fare clic su Calcola pianificazione ammortamento che deve essere compilata nel modulo
