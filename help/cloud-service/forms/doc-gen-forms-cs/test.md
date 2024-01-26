---
title: Testare la soluzione
description: Esegui Main.java per testare la soluzione
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 54
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Importa progetto Eclipse

Scarica e decomprimi il file [file zip](./assets/aem-forms-cs-doc-gen.zip)

Avvia Eclipse e importa il progetto in Eclipse Il progetto include i seguenti file nella cartella delle risorse:

* FileDati1,FileDati2 e FileDati3: file di dati XML di esempio da unire al modello per generare il file PDF finale
* custom_fonts.xdp - Modello XDP.
* service_token.json - Sostituire il contenuto di questo file con le credenziali specifiche dell’account
* options.json - Le opzioni specificate in questo file vengono utilizzate per impostare le proprietà del file PDF generato dall’API

![resources-file](./assets/resource-files.png)

## Testare la soluzione

* Copia e incolla le credenziali del servizio nel file di risorse service_token.json nel progetto.
* Aprire il file DocumentGeneration.java e specificare la cartella in cui si desidera salvare i file PDF generati
* Apri Main.java. Imposta il valore della variabile postURL in modo che punti all’istanza.
* Eseguire l’applicazione Java Main.java come

>[!NOTE]
> La prima volta che esegui il programma Java, riceverai un errore HTTP 403. Per superare questo problema, assicurati di somministrare [autorizzazioni appropriate per l’utente dell’account tecnico in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Utenti AEM Forms** è il ruolo che ho utilizzato per questo corso.
