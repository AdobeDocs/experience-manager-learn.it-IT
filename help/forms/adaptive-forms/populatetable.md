---
title: Popolare tabella modulo adattivo
description: Popolare la tabella del modulo adattivo con i risultati delle chiamate al servizio del modello dati modulo
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 45
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Popolare la tabella del modulo adattivo con i risultati della chiamata al servizio del modello di dati del modulo

[Il modulo live è ospitato qui](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
In questo articolo esaminiamo come popolare la tabella dei moduli adattivi recuperando i dati dalla chiamata al servizio del modello di dati del modulo. Verrà creato un piano di ammortamento in una tabella che elenca ogni pagamento regolare di un mutuo nel tempo. I risultati dell’ammortamento vengono restituiti dal modello dati del modulo. Il servizio del modello dati modulo viene richiamato sull’evento di clic del pulsante calcola come mostrato nella schermata. I parametri di input e output della chiamata al servizio sono mappati in modo appropriato, come mostrato nella schermata. L&#39;output viene mappato alle colonne di Row1
![clickevent](assets/amortization.PNG)

Row1 è configurato per l’aumento di dimensione a seconda dei dati restituiti dalla chiamata del servizio. Osserva le impostazioni di ripetizione specificate qui. Il valore -1 indica un numero illimitato di righe nella tabella
![Riga1](assets/rowconfiguration.PNG)

## Distribuisci sul server

[Installa Tomcat come specificato qui](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Distribuisci il file SampleRest.war contenuto in questo file zip nel tuo Tomcat](assets/sample-rest.zip)
[Installare le risorse](assets/amortizationschedule.zip) tramite Gestione pacchetti di AEM
[Aprire il modulo di pianificazione ammortamento](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Immetti il valore appropriato e fai clic su calcola
La pianificazione dell&#39;ammortamento dovrebbe essere compilata nel modulo
