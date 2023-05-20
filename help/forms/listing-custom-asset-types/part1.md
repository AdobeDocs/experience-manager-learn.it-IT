---
title: Registrazione di tipi di risorse personalizzati
seo-title: Registering Custom Asset Types
description: Abilitazione dei tipi di risorse personalizzati per l’inserimento nell’elenco in AEMForms Portal
seo-description: Enabling custom asset types for listing in AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 2%

---

# Registrazione di tipi di risorse personalizzati {#registering-custom-asset-types}

Abilitazione dei tipi di risorse personalizzati per l’inserimento nell’elenco in AEMForms Portal

>[!NOTE]
>
>Verificare che sia installato AEM 6.3 con SP1 e il corrispondente componente aggiuntivo AEM Forms. Questa funzione funziona solo con AEM Forms 6.3 SP1 e versioni successive

## Specifica percorso base {#specify-base-path}

Il percorso base è il percorso dell’archivio di livello superiore che include tutte le risorse che un utente potrebbe voler elencare nel componente Ricerca e lister. Se lo desideri, l’utente può anche configurare posizioni specifiche all’interno del percorso base dalla finestra di dialogo di modifica del componente, in modo che la ricerca venga attivata su posizioni specifiche anziché cercare tutti i nodi all’interno del percorso base. Per impostazione predefinita, il percorso di base viene utilizzato come criterio del percorso di ricerca per recuperare le risorse, a meno che l’utente non configuri un set di percorsi specifici dall’interno di questa posizione. È importante avere un valore ottimale di questo percorso per effettuare una ricerca performante. Il valore predefinito del percorso di base rimarrà invariato **_/content/dam/formsanddocuments_** perché tutte le risorse AEM Forms risiedono in **_/content/dam/formsanddocuments._**

Passaggi per configurare il percorso di base

1. Accedi a crx
1. Accedi a **/libs/fd/fp/extensions/querybuilder/basepath**

1. Fai clic su &quot;Sovrapponi nodo&quot; nella barra degli strumenti
1. Assicurati che la posizione della sovrapposizione sia &quot;/apps/&quot;
1. Fare clic su Ok
1. Fai clic su Salva
1. Passa alla nuova struttura creata in **/apps/fd/fp/extensions/querybuilder/basepath**

1. Modifica il valore della proprietà percorso in **&quot;/content/dam&quot;**
1. Fai clic su Salva

Specificando la proprietà del percorso a **&quot;/content/dam&quot;** si sta fondamentalmente impostando Percorso base su /content/dam. Per verificare questo aspetto, apri il componente Ricerca ed elenco.

![basepath](assets/basepath.png)

## Registrare i tipi di risorse personalizzati {#register-custom-asset-types}

È stata aggiunta una nuova scheda (Elenco risorse) nel componente Ricerca e lister. Questa scheda elenca i tipi di risorse predefiniti e i tipi di risorse aggiuntivi che puoi configurare. Per impostazione predefinita, sono elencati i seguenti tipi di risorse

1. Moduli adattivi
1. Modelli di modulo
1. PDF forms
1. Documento (PDF statici)

**Passaggi per registrare il tipo di risorsa personalizzato**

1. Crea nodo di sovrapposizione di **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Imposta la posizione di sovrapposizione su &quot;/apps&quot;
1. Passa alla nuova struttura creata in `/apps/fd/fp/extensions/querybuilder/assettypes`

1. In questa posizione, crea un nodo &quot;nt:unstructured&quot; per il tipo da registrare, quindi assegna al nodo il nome **file mp4. Aggiungi le due seguenti proprietà a questo nodo mp4files**

   1. Aggiungi la proprietà jcr:title per specificare il nome visualizzato del tipo di risorsa. Impostate il valore di jcr:title su &quot;File Mp4&quot;.
   1. Aggiungi la proprietà &quot;type&quot; e impostane il valore su &quot;videos&quot;. Questo è il valore che utilizziamo nel nostro modello per elencare le risorse del tipo video. Salva le modifiche.

1. Crea un nodo di tipo &quot;nt:unstructured&quot; in mp4files. Denomina questo nodo &quot;criteri di ricerca&quot;
1. Aggiungi uno o più filtri nei criteri di ricerca. Supponiamo che, se l&#39;utente vuole avere un filtro di ricerca per elencare i file mp4il cui tipo di mime è &quot;video/mp4&quot;, è possibile farlo qui
1. Crea un nodo di tipo &quot;nt:unstructured&quot; sotto i criteri di ricerca del nodo. Denomina questo nodo &quot;filetypes&quot;
1. Aggiungi le seguenti 2 proprietà a questo nodo &quot;filetypes&quot;

   1. name: ./jcr:content/metadata/dc:format
   1. valore: video/mp4

1. Ciò significa che le risorse con la proprietà dc:format uguale a video/mp4 sono considerate un tipo di risorsa &quot;Video Mp4&quot;. Per i criteri di ricerca è possibile utilizzare qualsiasi proprietà elencata nel nodo &quot;jcr:content/metadata&quot;

1. **Assicurati di salvare i tuoi dati**

Dopo aver eseguito i passaggi precedenti, il nuovo tipo di risorsa (File Mp4) inizierà a essere visualizzato nell’elenco a discesa dei tipi di risorsa del componente Ricerca ed Elenco come mostrato di seguito

![mp4files](assets/mp4files.png)

[In caso di problemi nel funzionamento di questa operazione, puoi importare il pacchetto seguente.](assets/assettypeskt1.zip) Nel pacchetto sono definiti due tipi di risorse personalizzate. File Mp4 e documenti Word. Suggerisci di dare un&#39;occhiata al **/apps/fd/fp/extensions/querybuilder/assettypes**

[Installare il pacchetto customeportal](assets/customportalpage.zip). Questo pacchetto contiene una pagina di esempio per il portale. Questa pagina è utilizzata nella parte 2 di questa esercitazione
