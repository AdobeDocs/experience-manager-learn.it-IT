---
title: Verificare la soluzione
description: Esegui ExecuteAssemblerService.java per testare la soluzione
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# Importa progetto Eclipse

* Scarica e decomprimi il file [file zip](./assets/pdf-manipulation.zip)
* Avvia Eclipse e importa il progetto in Eclipse
* Il progetto include le seguenti cartelle nella cartella delle risorse:
   * ddxFiles - Questa cartella contiene il file ddx per descrivere l&#39;output che si desidera generare
   * pdffiles - Questa cartella contiene i file pdf che si desidera assemblare e i file pdf per testare le utitilites PDFA
   * credenziali - Questa cartella contiene il file pdfa-options.json

![file di risorse](./assets/resources.png)

## Test dell’assemblaggio di file PDF

* Copia e incolla le credenziali del servizio nel file di risorsa service_token.json nel progetto.
* Apri il file AssemblePDFFiles.java e specifica la cartella in cui salvare i file PDF generati
* Apri ExecuteAssemblerService.java. Imposta il valore della variabile _AEM_FORMS_CS_ per puntare alla tua istanza.
* Rimuovi il commento dalle righe appropriate per testare l’assemblaggio di due o più file PDF
* Esegui l&#39;applicazione ExecuteAssemblerService.java come applicazione java

### Utilità di test PDFA

* Copia e incolla le credenziali del servizio nel file di risorsa service_token.json nel progetto.
* Apri il file PDFAUtilities.java e specifica la cartella in cui salvare i file PDF generati.
* Apri ExecuteAssemblerService.java. Imposta il valore della variabile _AEM_FORMS_CS_ per puntare alla tua istanza.
* Rimuovi il commento dalle righe appropriate per testare le operazioni PDFA.
* Esegui l&#39;applicazione ExecuteAssemblerService.java come applicazione java.



>[!NOTE]
> La prima volta che esegui il programma java riceverai un errore HTTP 403. Per superare questo, assicurati di [autorizzazioni appropriate per l’utente dell’account tecnico in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Utenti AEM Forms** è il ruolo che ho utilizzato per questo corso.
