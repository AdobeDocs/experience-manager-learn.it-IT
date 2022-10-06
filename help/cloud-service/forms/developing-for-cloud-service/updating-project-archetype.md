---
title: Aggiornamento del progetto del servizio cloud con l’archetipo più recente
description: Aggiornamento del progetto del servizio cloud AEM Forms con l’archetipo più recente
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---

# Migrazione dal vecchio archetipo di aem

Per aggiornare il progetto AEM Forms esistente con l’archetipo maven più recente, dovrai copiare manualmente il codice/configurazioni ecc. dal vecchio progetto al nuovo progetto.

Sono stati seguiti i seguenti passaggi per migrare il progetto creato utilizzando archetype 30 al progetto archetype 33

## Crea un progetto Maven utilizzando l’archetipo più recente

* Apri il prompt dei comandi e passa a c:\cloudmanager
* Crea un progetto maven utilizzando l’archetipo più recente.
* Copia e incolla il contenuto del [file di testo](assets/creating-maven-project.txt) nella finestra del prompt dei comandi. Potrebbe essere necessario modificare il DarchetypeVersion=33 a seconda del [versione più recente](https://github.com/adobe/aem-project-archetype/releases). Archetype 33 include nuovi temi AEM Forms.
Poiché stiamo creando il nuovo progetto maven nella cartella cloudmanager che dispone già di un progetto aem-banking-application, è necessario modificare il **DartifactId** dall&#39;applicazione aem-banking a qualcosa di diverso. Ho usato aem-banking-application1 per questo articolo.

>[!NOTE]
>
>Se si distribuisce questo nuovo progetto così com’è, l’istanza del servizio cloud non avrà HandleFormSubmission e SubmitToAEMServlet. Questo perché ogni volta che distribuisci un progetto utilizzando Cloud Manager qualsiasi elemento nel `/apps` la cartella viene eliminata e sovrascritta.

## Copia il codice java

Una volta creato correttamente il progetto, puoi iniziare a copiare il codice/le configurazioni ecc. dal vecchio progetto a questo nuovo progetto

* Copia il servlet HandleFormSubmission da ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
a

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copiare CustomSubmit da
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` dall’applicazione aem-banking al progetto aem-banking-application1

* importare il nuovo progetto in IntelliJ

* Aggiorna il filter.xml nel modulo ui.apps del progetto aem-banking-application1 per includere la seguente riga
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Dopo aver copiato tutto il codice nel nuovo progetto, puoi inviarlo a cloud manager.

>[!NOTE]
>
>Per sincronizzare il contenuto (Adaptive Forms, Form Data Model, ecc.) nel nuovo progetto, dovrai creare la struttura di cartelle appropriata nel progetto IntelliJ e quindi sincronizzare il progetto IntelliJ con la tua istanza AEM utilizzando il comando Get dello strumento repo.
