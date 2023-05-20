---
title: Test della soluzione del kit di benvenuto
description: Implementazione delle risorse della soluzione per testare la soluzione
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Distribuire e testare le risorse di esempio

[Installare il pacchetto del kit di benvenuto](assets/welcomekit.zip). Questo pacchetto contiene il modello della pagina, il componente personalizzato per elencare le risorse, il flusso di lavoro di esempio, il modello e-mail e alcuni documenti PDF di esempio da includere nel kit di benvenuto.
[Installare il bundle welcome-kit](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Questo bundle contiene il codice per creare la pagina e la classe Java per restituire le risorse da visualizzare nella pagina web.
[Installare il modulo adattivo di esempio](assets/account-openeing-form.zip)
Configura il servizio di posta Day CQ. Il flusso di lavoro invia il collegamento del kit di benvenuto al mittente del modulo che necessita di una configurazione SMTP corretta.
Configurare il componente Invia e-mail del flusso di lavoro in base alle tue esigenze
[Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Inserisci i tuoi dati e seleziona uno o più fondi comuni di investimento e invia il modulo Dovresti ricevere un’e-mail con un collegamento al kit di benvenuto
