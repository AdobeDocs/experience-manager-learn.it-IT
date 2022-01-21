---
title: Aggiornamento del progetto del servizio cloud con l’archetipo più recente
description: Aggiornamento del progetto del servizio cloud AEM Forms con l’archetipo più recente
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9534
source-git-commit: cea9a9dc003b76369db1b7fedb9549062885258d
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 1%

---

# Migrating from old aem archetype

Per aggiornare il progetto AEM Forms esistente con l’archetipo maven più recente, dovrai copiare manualmente il codice/configurazioni ecc. dal vecchio progetto al nuovo progetto.

The following steps were followed to migrate the project created using archetype 30 to archetype 33 project

## Crea un progetto Maven utilizzando l’archetipo più recente

* Apri il prompt dei comandi e passa a c:\cloudmanager
* Create maven project using the latest archetype.
* Copia e incolla il contenuto del [file di testo](assets/creating-maven-project.txt) nella finestra del prompt dei comandi. Potrebbe essere necessario modificare il DarchetypeVersion=33 a seconda del [versione più recente](https://github.com/adobe/aem-project-archetype/releases). Archetype 33 includes new AEM Forms themes.
Poiché stiamo creando il nuovo progetto maven nella cartella cloudmanager che dispone già di un progetto aem-banking-application, è necessario modificare il **DartifactId** dall&#39;applicazione aem-banking a qualcosa di diverso. Ho usato aem-banking-application1 per questo articolo.

>[!NOTE]
>
>Se si distribuisce questo nuovo progetto così com’è, l’istanza del servizio cloud non avrà HandleFormSubmission e SubmitToAEMServlet. Questo perché ogni volta che distribuisci un progetto utilizzando cloud manager, qualsiasi elemento presente nella cartella delle app verrà eliminato e sovrascritto.

## Copia il codice java

Una volta creato correttamente il progetto, puoi iniziare a copiare il codice/le configurazioni ecc. dal vecchio progetto a questo nuovo progetto

* Copia il servlet HandleFormSubmission da ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
a

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copiare CustomSubmit da
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` dall’applicazione aem-banking al progetto aem-banking-application1

* import the new project into IntelliJ

* Aggiorna il filter.xml nel modulo ui.apps del progetto aem-banking-application1 per includere la seguente riga
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Dopo aver copiato tutto il codice nel nuovo progetto, puoi inviarlo a cloud manager.

>[!NOTE]
>
>Per sincronizzare il contenuto (Adaptive Forms, Form Data Model, ecc.) nel nuovo progetto, dovrai creare la struttura di cartelle appropriata nel progetto IntelliJ e quindi sincronizzare il progetto IntelliJ con la tua istanza AEM utilizzando il comando Get dello strumento repo.
