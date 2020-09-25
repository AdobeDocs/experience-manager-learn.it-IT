---
title: ' AEM Forms con Marketo(Parte 4)'
seo-title: ' AEM Forms con Marketo(Parte 4)'
description: Esercitazione per integrare  AEM Forms con Marketo mediante  AEM Forms Form Data Model.
seo-description: Esercitazione per integrare  AEM Forms con Marketo mediante  AEM Forms Form Data Model.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# Creazione di moduli adattivi con il modello dati del modulo

Il passaggio successivo consiste nel creare un modulo adattivo e nel basarlo sul modello dati del modulo creato nel passaggio precedente.
L’utente immetterà l’ID lead e verrà richiamato il servizio Marketo per recuperare i lead per ID. I risultati dell&#39;operazione del servizio vengono quindi mappati sui campi appropriati dell&#39;Forms adattivo.

1. Creare un modulo adattivo e basarlo su &quot;Modello modulo vuoto&quot;, associarlo al modello dati modulo creato nel passaggio precedente.
1. Aprire il modulo in modalità di modifica
1. Trascinare nel modulo adattivo un componente TextField e un componente Pannello. Impostate il titolo del componente TextField &quot;Enter Lead Id&quot; e impostatene il nome su &quot;LeadId&quot;
1. Trascinare 2 componenti TextField sul componente Pannello
1. Impostare Nome e Titolo dei due componenti Campo di testo come Nome e Cognome
1. Configurate il componente Pannello in modo che sia un componente ripetibile impostando il valore Minimo su 1 e Massimo su -1. Questo è richiesto perché il servizio Marketo restituisce un array di oggetti lead e per visualizzare i risultati è necessario disporre di un componente ripetibile. Tuttavia, in questo caso, verrà restituito un solo oggetto Lead perché gli oggetti Lead vengono cercati in base all&#39;ID.
1. Crea una regola nel campo LeadId come mostrato nell&#39;immagine seguente
1. Visualizzare l’anteprima del modulo e immettere un ID lead valido nel campo LeadID e tabulazione. I campi Nome e Cognome devono essere compilati con i risultati della chiamata al servizio.

Nella schermata seguente sono illustrate le impostazioni dell&#39;editor di regole

![ruleeditor](assets/ruleeditor.jfif)
