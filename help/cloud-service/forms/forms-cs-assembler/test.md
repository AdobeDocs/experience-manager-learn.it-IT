---
title: Verificare la soluzione
description: Esegui ExecuteAssemblerService.java per testare la soluzione
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# Importa progetto Eclipse

* Scarica e decomprimi il file [file zip](./assets/pdf-manipulation.zip)
* Avvia Eclipse e importa il progetto in Eclipse
* Il progetto include le seguenti cartelle nella cartella delle risorse:
   * ddxFiles - Questa cartella contiene il file ddx per descrivere l&#39;output che si desidera generare
   * pdffiles - Questa cartella contiene i file pdf che si desidera assemblare

![file di risorse](./assets/resources.png)

## Verificare la soluzione

* Copia e incolla le credenziali del servizio nel file di risorsa service_token.json nel progetto.
* Apri il file AssemblePDFFiles.java e specifica la cartella in cui salvare i file PDF generati
* Apri ExecuteAssemblerService.java. Imposta il valore della variabile assembleURL in modo che punti all&#39;istanza.
* Esegui l&#39;applicazione ExecuteAssemblerService.java come applicazione java

>[!NOTE]
> La prima volta che esegui il programma java riceverai un errore HTTP 403. Per superare questo, assicurati di [autorizzazioni appropriate per l’utente dell’account tecnico in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Utenti AEM Forms** è il ruolo che ho utilizzato per questo corso.
