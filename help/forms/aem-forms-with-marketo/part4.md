---
title: AEM Forms con Marketo (parte 4)
seo-title: AEM Forms con Marketo (parte 4)
description: Esercitazione per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
seo-description: Esercitazione per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: '"Moduli adattivi, modello dati modulo"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---


# Creazione di moduli adattivi tramite il modello dati del modulo

Il passaggio successivo consiste nel creare un Modulo adattivo e basarlo sul modello dati del modulo creato nel passaggio precedente.
L’utente immetterà l’ID lead e verrà richiamato il servizio Marketo all’uscita del tag per ottenere i lead per ID. I risultati dell’operazione del servizio vengono quindi mappati sui campi appropriati dei Moduli adattivi.

1. Creare un modulo adattivo e basarlo su &quot;Modello di modulo vuoto&quot;, associarlo al modello di dati del modulo creato nel passaggio precedente.
1. Apri il modulo in modalità di modifica
1. Trascina un componente TextField e un componente Panel sul modulo adattivo. Imposta il titolo del componente TextField &quot;Enter Lead Id&quot; (Inserisci ID lead) e imposta il suo nome su &quot;LeadId&quot;
1. Trascina 2 componenti TextField sul componente Pannello
1. Impostare il Nome e il Titolo dei due componenti del campo di testo come Nome e Cognome
1. Configura il componente Pannello come componente ripetibile impostando Minimo su 1 e Massimo su -1. Questo è necessario perché il servizio Marketo restituisce una matrice di oggetti lead e per visualizzare i risultati è necessario disporre di un componente ripetibile. Tuttavia, in questo caso, riceveremo indietro un solo oggetto Lead perché stiamo eseguendo ricerche sugli oggetti Lead in base al relativo ID.
1. Crea una regola sul campo LeadId come mostrato nell’immagine seguente
1. Visualizzare l’anteprima del modulo e immettere un ID lead valido nel campo LeadID e uscire dalla scheda. I campi Nome e Cognome devono essere compilati con i risultati della chiamata del servizio.

La schermata seguente spiega le impostazioni dell’editor di regole

![editor di regole](assets/ruleeditor.jfif)
