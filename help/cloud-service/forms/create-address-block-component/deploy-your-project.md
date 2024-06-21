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
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Distribuire il progetto

Prima di iniziare la distribuzione del progetto al Cloud Service AEM Forms, si consiglia di implementarlo nell’istanza cloud locale di AEM Forms.

## Sincronizzare le modifiche con il progetto AEM

Avvia IntelliJ e passa alla cartella adaptiveForm sotto il ``ui.apps`` come mostrato di seguito
![intellij](assets/intellij.png)

Fai clic con il pulsante destro del mouse su ``adaptiveForm`` e seleziona Nuovo | Pacchetto Assicurati di aggiungere il nome **addressblock** alla confezione

Fai clic con il pulsante destro del mouse sul pacchetto appena creato ``addressblock`` e seleziona ``repo | Get Command`` come mostrato di seguito
![repo-sync](assets/sync-repo.png)

Questo dovrebbe sincronizzare il progetto con l’istanza AEM Forms locale predisposta per il cloud. Puoi verificare il file .content.xml per confermare le proprietà
![dopo la sincronizzazione](assets/after-sync.png)

## Distribuire il progetto nell’istanza locale

Avvia una nuova finestra del prompt dei comandi, passa alla cartella principale del progetto e crea il progetto utilizzando il comando mostrato di seguito
![distribuire](assets/build-project.png)

Una volta implementato correttamente il progetto, il componente Indirizzo può ora essere utilizzato in un modulo adattivo

## Distribuire il progetto in un ambiente cloud

Se tutto funziona nell&#39;ambiente di sviluppo locale, il passaggio successivo consiste nell&#39;implementare [istanza cloud tramite cloud manager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



