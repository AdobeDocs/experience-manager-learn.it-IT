---
title: Aggiornamento del progetto del servizio cloud con l’archetipo più recente
description: Aggiornamento del progetto AEM Forms Cloud Service con l’archetipo più recente
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Migrazione dal vecchio archetipo aem

Per aggiornare il progetto AEM Forms esistente con l’archetipo Maven più recente, dovrai copiare manualmente il codice/le configurazioni, ecc., dal vecchio progetto al nuovo.

Sono stati seguiti i seguenti passaggi per migrare il progetto creato utilizzando il progetto archetipo 30 a archetipo 33

## Crea un progetto Maven utilizzando l’archetipo più recente

* Apri il prompt dei comandi e passa a c:\cloudmanager
* Crea un progetto Maven utilizzando l’archetipo più recente.
* Copiare e incollare il contenuto del [file di testo](assets/creating-maven-project.txt) nella finestra del prompt dei comandi. Potrebbe essere necessario modificare DarchetypeVersion=33 a seconda della [versione più recente](https://github.com/adobe/aem-project-archetype/releases). Archetipo 33 include nuovi temi AEM Forms.
Poiché stiamo creando il nuovo progetto Maven nella cartella cloudmanager che dispone già del progetto dell&#39;applicazione aem-banking, è necessario modificare **DartifactId** da aem-banking-application a un altro progetto. Ho usato aem-banking-application1 per questo articolo.

>[!NOTE]
>
>Se distribuisci questo nuovo progetto così come è l’istanza del servizio cloud, HandleFormSubmission e SubmitToAEMServlet non saranno disponibili. Questo perché ogni volta che si distribuisce un progetto utilizzando Cloud Manager, qualsiasi elemento nella cartella `/apps` viene eliminato e sovrascritto.

## Copia il codice Java

Una volta creato correttamente il progetto, puoi iniziare a copiare codice/configurazioni, ecc., dal vecchio progetto al nuovo

* Copia il servlet HandleFormSubmission da ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
a
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copia l’oggetto CustomSubmit da
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` dal progetto aem-banking-application al progetto aem-banking-application1

* importa il nuovo progetto in IntelliJ

* Aggiorna il file filter.xml nel modulo ui.apps del progetto aem-banking-application1 per includere la riga seguente
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Dopo aver copiato tutto il codice nel nuovo progetto, puoi inviare il progetto a Cloud Manager.

>[!NOTE]
>
>Per sincronizzare il contenuto (Forms adattivo, Modello dati modulo, ecc.) nel nuovo progetto, è necessario creare la struttura di cartelle appropriata nel progetto IntelliJ e quindi sincronizzare il progetto IntelliJ con l&#39;istanza AEM utilizzando il comando Get dello strumento repository.
