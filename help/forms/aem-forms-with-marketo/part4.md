---
title: AEM Forms con Marketo (parte 4)
description: Esercitazione per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# Creazione di moduli adattivi tramite il modello dati del modulo

Il passaggio successivo consiste nel creare un Modulo adattivo e basarlo sul modello dati del modulo creato nel passaggio precedente.
L’utente immette l’ID lead e si richiama il servizio Marketo all’arresto del tag per ottenere i lead per ID. I risultati dell&#39;operazione del servizio vengono quindi mappati sui campi appropriati di Adaptive Forms.

1. Creare un modulo adattivo e basarlo su &quot;Modello di modulo vuoto&quot;, associarlo al modello di dati del modulo creato nel passaggio precedente.
1. Apri il modulo in modalità di modifica
1. Trascina un componente TextField e un componente Panel sul modulo adattivo. Imposta il titolo del componente TextField &quot;Enter Lead Id&quot; (Inserisci ID lead) e imposta il suo nome su &quot;LeadId&quot;
1. Trascina 2 componenti TextField sul componente Pannello
1. Impostare il Nome e il Titolo dei due componenti del campo di testo come Nome e Cognome
1. Configura il componente Pannello come componente ripetibile impostando Minimo su 1 e Massimo su -1. Questo è necessario perché il servizio Marketo restituisce una matrice di oggetti lead e per visualizzare i risultati è necessario disporre di un componente ripetibile. Tuttavia, in questo caso, stiamo ottenendo indietro un solo oggetto Lead perché stiamo cercando gli oggetti Lead in base al suo ID.
1. Crea una regola sul campo LeadId come mostrato nell’immagine seguente
1. Visualizzare l’anteprima del modulo e immettere un ID lead valido nel campo LeadID e uscire dalla scheda. I campi Nome e Cognome devono essere compilati con i risultati della chiamata del servizio.

La schermata seguente spiega le impostazioni dell’editor di regole

![editor di regole](assets/ruleeditor.jfif)

## Debugging

Se utilizzi i bundle forniti con questo articolo, puoi abilitare [registri di debug](http://localhost:4502/system/console/slinglog) per le classi seguenti:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
