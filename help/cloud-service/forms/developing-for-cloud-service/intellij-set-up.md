---
title: Installazione di IntelliJ community edition
description: Installare e importare il progetto AEM in IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
duration: 65
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---

# Installazione di IntelliJ

Installa [edizione della community IntelliJ](https://www.jetbrains.com/idea/download/#section=windows). Durante l&#39;installazione è possibile accettare le impostazioni predefinite, se consigliate.

## Importare il progetto AEM

* Avvia IntelliJ
* Importa il progetto AEM creato nel passaggio precedente. Dopo l’importazione del progetto, lo schermo avrà un aspetto simile al seguente ![aem-banking-app](assets/aem-banking-app.png). In genere, puoi utilizzare i sottoprogetti core, ui.apps, ui.config e ui.content.
* Se la finestra Maven e il terminale non sono visualizzati, passare alla finestra Visualizza->Strumenti e selezionare Maven e Terminal.

## Aggiungere il modulo font

Se desideri utilizzare font personalizzati nel file PDF, devi inviarli all’istanza di AEM Forms CS. Segui i seguenti passaggi

* Crea una cartella denominata **font** in C:\CloudManager\aem-banking-application
* Estrai il contenuto di [font.zip](assets/fonts.zip) nella cartella dei tipi di carattere appena creata
* Nel modulo font sono inclusi alcuni font personalizzati.È possibile aggiungere i font personalizzati della propria organizzazione alla cartella C:\CloudManager\aem-banking-application\fonts\src\main\resources del modulo font
* Apri il file C:\CloudManager\aem-banking-application\pom.xml
* Aggiungi la seguente riga  ```<module>fonts</module>``` nella sezione moduli del file pom.xml
* Salvare pom.xml
* Aggiorna il progetto dell&#39;applicazione aem-banking in IntelliJ

Struttura del progetto con modulo font
![fonts-module](assets/fonts-module.png)

Modulo font incluso nel POM dei progetti
![font-pom](assets/fonts-module-pom.png)

## Passaggi successivi

[Configurazione Git](./setup-git.md)
