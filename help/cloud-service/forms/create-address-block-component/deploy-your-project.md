---
title: Creazione del componente indirizzo
description: Creazione di un nuovo componente core indirizzo nel Cloud Service AEM Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: a12b1778413079646814cb25567abfc26a429340
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Distribuire il progetto

Prima di iniziare la distribuzione del progetto al Cloud Service AEM Forms, si consiglia di implementarlo nell’istanza cloud locale di AEM Forms.

## Sincronizzare le modifiche con il progetto AEM

Avviare IntelliJ e passare alla cartella adaptiveForm nella cartella ``ui.apps`` come illustrato di seguito
![intellij](assets/intellij.png)

Fare clic con il pulsante destro del mouse sul nodo ``adaptiveForm`` e selezionare Nuovo | Pacchetto
Assicurarsi di aggiungere il nome **addressblock** al pacchetto

Fare clic con il pulsante destro del mouse sul pacchetto appena creato ``addressblock`` e selezionare ``repo | Get Command`` come illustrato di seguito
![repo-sync](assets/sync-repo.png)

Questo dovrebbe sincronizzare il progetto con l’istanza AEM Forms locale predisposta per il cloud. Puoi verificare il file .content.xml per confermare le proprietà
![dopo-sincronizzazione](assets/after-sync.png)

## Distribuire il progetto nell’istanza locale

Avvia una nuova finestra del prompt dei comandi, passa alla cartella principale del progetto e crea il progetto utilizzando il comando mostrato di seguito
![distribuisci](assets/build-project.png)

Una volta implementato correttamente il progetto,
Il componente Indirizzo può ora essere utilizzato in un modulo adattivo

## Distribuire il progetto in un ambiente cloud

Se tutto si presenta correttamente nell&#39;ambiente di sviluppo locale, il passaggio successivo consiste nella distribuzione all&#39;istanza cloud [tramite Cloud Manager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
