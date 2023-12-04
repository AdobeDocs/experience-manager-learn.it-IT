---
title: AEM Forms con Marketo (Parte 4)
description: Tutorial per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 95
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Creazione di un modulo adattivo tramite il modello dati del modulo

Il passaggio successivo consiste nel creare un modulo adattivo e basarlo sul modello dati modulo creato nel passaggio precedente.
L’utente immette l’ID lead e all’uscita dal servizio Marketo viene richiamato per ottenere i lead per ID. I risultati dell’operazione di servizio vengono quindi mappati sui campi appropriati del Forms adattivo.

1. Crea un modulo adattivo e basalo su &quot;Modello di modulo vuoto&quot;, associalo al modello di dati del modulo creato nel passaggio precedente.
1. Apri il modulo in modalità di modifica
1. Trascina e rilascia sul modulo adattivo un componente TextField e un componente Panel. Imposta il titolo del componente TextField &quot;Enter Lead Id&quot; e il relativo nome su &quot;LeadId&quot;
1. Trascina 2 componenti TextField sul componente Pannello
1. Impostare Nome e Titolo dei due componenti Textfield come Nome e Cognome
1. Configura il componente Pannello come componente ripetibile impostando Minimo su 1 e Massimo su -1. Questa operazione è necessaria in quanto il servizio Marketo restituisce un array di oggetti lead ed è necessario disporre di un componente ripetibile per visualizzare i risultati. Tuttavia, in questo caso viene restituito un solo oggetto Lead perché la ricerca viene eseguita sugli oggetti Lead in base al relativo ID.
1. Crea una regola nel campo LeadId come illustrato nell’immagine seguente
1. Visualizza l&#39;anteprima del modulo e immetti un ID lead valido nel campo ID lead e tabulazione. I campi Nome e Cognome devono essere compilati con i risultati della chiamata al servizio.

La schermata seguente spiega le impostazioni dell’editor di regole

![editor di regole](assets/ruleeditor.jfif)

## Debugging

Se utilizzi i bundle forniti con questo articolo, potrebbe essere utile abilitare [registri di debug](http://localhost:4502/system/console/slinglog) per le seguenti classi:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## Complimenti

Hai integrato correttamente AEM Forms con Marketo utilizzando AEM Forms Form Data Model.