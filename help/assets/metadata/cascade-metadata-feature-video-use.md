---
title: Utilizzo dei metadati a cascata in  AEM Assets
seo-title: Utilizzo dei metadati a cascata in  AEM Assets
description: La gestione avanzata dei metadati consente agli utenti di creare regole per i campi CSS per creare relazioni contestuali tra i metadati in  AEM Assets. Il video seguente illustra nuove regole dinamiche per requisiti di campo, visibilità e opzioni contestuali. Il video descrive inoltre i passaggi necessari affinché un amministratore possa applicare queste regole a uno schema di metadati personalizzato.
seo-description: La gestione avanzata dei metadati consente agli utenti di creare regole per i campi CSS per creare relazioni contestuali tra i metadati in  AEM Assets. Il video seguente illustra nuove regole dinamiche per requisiti di campo, visibilità e opzioni contestuali. Il video descrive inoltre i passaggi necessari affinché un amministratore possa applicare queste regole a uno schema di metadati personalizzato.
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# Utilizzo dei metadati a cascata in  AEM Assets{#using-cascading-metadata-in-aem-assets}

La gestione avanzata dei metadati consente agli utenti di creare regole per i campi CSS per creare relazioni contestuali tra i metadati in  AEM Assets. Il video seguente illustra nuove regole dinamiche per requisiti di campo, visibilità e opzioni contestuali. Il video descrive inoltre i passaggi necessari affinché un amministratore possa applicare queste regole a uno schema di metadati personalizzato.

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

Esistono tre set di regole dinamiche che possono essere attivati per un determinato campo di metadati:

1. **Requisito** : un campo può essere contrassegnato in modo dinamico come richiesto in base al valore di un altro campo a discesa.

2. **Visibilità** : i campi possono essere sempre visibili o solo visibili in base al valore di un altro campo a discesa.

3. **Scelte** : (applicabile solo ai campi a discesa) filtrare le scelte visualizzate in base al valore attualmente selezionato di un altro campo a discesa.

>[!NOTE]
>
>Le regole di sovrapposizione possono essere create SOLO in base ai valori di un campo a discesa. È possibile applicare tutti e tre i set di regole allo stesso campo di metadati, ma come procedura ottimale si consiglia di fare in modo che ciascun set di regole dipenda dallo stesso elenco a discesa di metadati.

Scarica [pacchetto di metadati personalizzati](assets/cascade-metadata-values-001.zip)

## Risorse aggiuntive{#additional-resources}

Schema metadati personalizzato creato in: `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. Il pacchetto AEM seguente applicherà alla cartella uno schema personalizzato: `/content/dam/we-retail/en/activities`:

