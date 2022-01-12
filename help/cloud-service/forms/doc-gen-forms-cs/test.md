---
title: Verificare la soluzione
description: Esegui Main.java per testare la soluzione
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: f712e86600ed18aee43187a5fb105324b14b7b89
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---


# Importa progetto Eclipse

Scarica e decomprimi il file [file zip](./assets/aem-forms-cs-doc-gen.zip)

Avvia Eclipse e importa il progetto in Eclipse Il progetto include i seguenti file nella cartella delle risorse:

* DataFile1, DataFile2 e DataFile3 - File di dati xml di esempio da unire con il modello per generare il file PDF finale
* custom_fonts.xdp - modello XDP.
* service_token.json - Dovrai sostituire il contenuto di questo file con le credenziali specifiche del tuo account
* options.json - Le opzioni specificate in questo file vengono utilizzate per impostare le proprietà del file PDF generato dall&#39;API

![file di risorse](./assets/resource-files.png)

## Verificare la soluzione

* Copia e incolla le credenziali del servizio nel file di risorsa service_token.json nel progetto.
* Apri il file DocumentGeneration.java e specifica la cartella in cui salvare i file PDF generati
* Apri Main.java. Imposta il valore della variabile postURL in modo che punti all&#39;istanza.
* Esegui l&#39;applicazione Main.java come java

>[!NOTE]
> La prima volta che esegui il programma java riceverai un errore HTTP 403. Per superare questo, assicurati di [autorizzazioni appropriate per l’utente dell’account tecnico in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Utenti AEM Forms** è il ruolo che ho utilizzato per questo corso.

