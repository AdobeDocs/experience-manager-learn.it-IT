---
title: 'Popolare tabella di moduli adattivi '
description: Compilare una tabella di moduli adattivi con i risultati delle vocazioni al servizio Modello dati modulo
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---


# Compilare una tabella di moduli adattivi con i risultati dell’invocazione a Form Data Model Service

[Il modulo live è ospitato ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
quiIn questo articolo esaminiamo la compilazione della tabella dei moduli adattivi recuperando i dati dalla chiamata al servizio del modello dati del modulo. Stiamo per creare un programma di ammortamento in una tabella che elenca ogni pagamento regolare su un mutuo nel tempo. I risultati dell’ammortamento vengono restituiti dal modello dati modulo. Il servizio di Form Data Model viene richiamato sull&#39;evento click del pulsante calculate , come mostrato nella schermata . I parametri di input e output della chiamata del servizio vengono mappati in modo appropriato come mostrato nella schermata. L&#39;output è mappato alle colonne della riga1
![clickevent](assets/amortization.PNG)

La riga1 è configurata in modo che cresca a seconda dei dati restituiti dalla chiamata del servizio. Osserva le impostazioni di ripetizione specificate qui. Il valore -1 indica un numero illimitato di righe nella tabella
![Riga1](assets/rowconfiguration.PNG)

## Distribuisci sul server

[Installa Tomcat come specificato ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[quiDistribuisci il file SampleRest.war contenuto in questo ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/assets/common-osgi-bundles/sample-rest.zip)
[file zipInstalla le risorse  ](assets/amortizationschedule.zip) utilizzando AEM gestore dei pacchetti 
[Apri il ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
modulo di pianificazione dell&#39;ammortamentoImmetti il valore appropriato e fai clic su calcola programma di ammortamento dovrebbe essere compilato nel modulo

