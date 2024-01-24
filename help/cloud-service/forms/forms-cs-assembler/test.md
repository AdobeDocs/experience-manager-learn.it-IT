---
title: Test della soluzione assembler Forms
description: Eseguire ExecuteAssemblerService.java per testare la soluzione
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 61
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Importa progetto Eclipse

* Scarica e decomprimi il file [file zip](./assets/pdf-manipulation.zip)
* Avvia Eclipse e importa il progetto in Eclipse
* Il progetto include le seguenti cartelle nella cartella delle risorse:
   * ddxFiles - Questa cartella contiene il file ddx per descrivere l&#39;output che si desidera generare
   * pdfile: questa cartella contiene i file PDF che desideri assemblare e i file PDF per testare le utilità PDFA
   * credenziali - Questa cartella contiene il file pdfa-options.json

![resources-file](./assets/resources.png)

## Test dei file PDF di assemblaggio

* Copia e incolla le credenziali del servizio nel file di risorse service_token.json nel progetto.
* Aprire il file AssemblePDFFiles.java e specificare la cartella in cui si desidera salvare i file PDF generati
* Aprire ExecuteAssemblerService.java. Imposta il valore della variabile _AEM_FORMS_CS_ per puntare all’istanza.
* Rimuovere il commento dalle righe appropriate per testare l&#39;assemblaggio di due o più file PDF
* Eseguire ExecuteAssemblerService.java come applicazione Java

### Test delle utilità PDFA

* Copia e incolla le credenziali del servizio nel file di risorse service_token.json nel progetto.
* Aprire il file PDFAUtilities.java e specificare la cartella in cui si desidera salvare i file PDF generati.
* Aprire ExecuteAssemblerService.java. Imposta il valore della variabile _AEM_FORMS_CS_ per puntare all’istanza.
* Rimuovi il commento dalle righe appropriate per testare le operazioni PDFA.
* Eseguire ExecuteAssemblerService.java come applicazione Java.



>[!NOTE]
> La prima volta che esegui il programma Java, riceverai un errore HTTP 403. Per superare questo problema, assicurati di somministrare [autorizzazioni appropriate per l’utente dell’account tecnico in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Utenti AEM Forms** è il ruolo che ho utilizzato per questo corso.
