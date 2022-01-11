---
title: Installazione dell'edizione della community IntelliJ
description: Installare e importare il progetto AEM in IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# Installazione di IntelliJ

Installa [Edizione comunitaria IntelliJ](https://www.jetbrains.com/idea/download/#section=windows). È possibile accettare le impostazioni predefinite durante l&#39;installazione.

## Importa progetto AEM

* Avvia IntelliJ
* Importa il progetto AEM creato nel passaggio precedente. Dopo l’importazione del progetto, la schermata dovrebbe avere un aspetto simile a questo ![aem-banking-app](assets/aem-banking-app.png). In genere lavorerai con i sottoprogetti core,ui.apps,ui.config e ui.content.
* Se non vedi la finestra Maven e terminale, vai a visualizzare->Finestra Strumenti e seleziona Maven e Terminal.

## Aggiungi il modulo dei font

Se desideri utilizzare font personalizzati nel file PDF, dovrai inviare i font personalizzati all’istanza AEM Forms CS. Segui i seguenti passaggi

* Crea una cartella denominata **font** in C:\CloudManager\aem-banking-application
* Estrarre il contenuto di [font.zip](assets/fonts.zip) nella cartella dei font appena creati
* Nel modulo dei font sono inclusi alcuni font personalizzati.Puoi aggiungere i font personalizzati della tua organizzazione a C:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts module
* Apri il file C:\CloudManager\aem-banking-application\pom.xml
* Aggiungi la seguente riga  ```<module>fonts</module>``` nella sezione moduli del file pom.xml
* Salva il tuo pom.xml
* Aggiornare il progetto aem-banking-application in IntelliJ

Struttura del progetto con modulo dei font
![fonts-module](assets/fonts-module.png)

Modulo Font incluso nel POM dei progetti
![font-pom](assets/fonts-module-pom.png)
