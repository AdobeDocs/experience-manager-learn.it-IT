---
title: Casella in entrata AEM
description: Personalizzare la casella in entrata aggiungendo nuove colonne in base ai dati del flusso di lavoro
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 5%

---

# Casella in entrata AEM

AEM Casella in entrata consolida le notifiche e le attività da vari componenti AEM, inclusi i flussi di lavoro Forms. Quando viene attivato un flusso di lavoro dei moduli contenente un passaggio dell’attività Assegna, l’applicazione associata viene elencata come un’attività nella casella in entrata dell’assegnatario.

L’interfaccia utente della casella in entrata fornisce viste elenco e calendario per visualizzare le attività. È inoltre possibile configurare le impostazioni di visualizzazione. È possibile filtrare le attività in base a vari parametri.

È possibile personalizzare un Casella in entrata Experience Manager per modificare il titolo predefinito di una colonna, riordinare la posizione di una colonna e visualizzare colonne aggiuntive in base ai dati di un flusso di lavoro.

>[!NOTE]
>
>Per personalizzare le colonne della casella in entrata, devi essere membro di amministratori o amministratori di flusso di lavoro

## Personalizzazione delle colonne

[Avvia AEM inbox](http://localhost:4502/aem/inbox)
Apri Admin Control facendo clic sul pulsante _Vista a elenco_ e quindi selezionare _Controllo amministratore_ come mostrato nella schermata sottostante

![admin-control](assets/open-customization.png)

Nell’interfaccia utente di personalizzazione delle colonne è possibile eseguire le operazioni seguenti

* Elimina colonne
* Riordinare le colonne
* Rinomina colonne

## Personalizzazione branding

Nella personalizzazione del branding puoi effettuare le seguenti operazioni

* Aggiungi il logo della tua organizzazione
* Personalizza il testo dell’intestazione
* Personalizzare il collegamento della guida
* Nascondi opzioni navigazione

![branding della casella in entrata](assets/branding-customization.PNG)

## Passaggi successivi

[Aggiungi colonna sposata](./add-married-column.md)
