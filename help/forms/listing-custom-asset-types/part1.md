---
title: Registrazione di tipi di risorse personalizzati
seo-title: Registering Custom Asset Types
description: Abilitazione dei tipi di risorse personalizzate per l’inserimento nell’elenco in AEMForms Portal
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 2%

---

# Registrazione di tipi di risorse personalizzati {#registering-custom-asset-types}

Abilitazione dei tipi di risorse personalizzate per l’inserimento nell’elenco in AEMForms Portal

>[!NOTE]
>
>Assicurati di avere installato AEM 6.3 con SP1 e il corrispondente componente aggiuntivo AEM Forms installato. Questa funzione funziona solo con AEM Forms 6.3 SP1 e versioni successive

## Specifica percorso di base {#specify-base-path}

Il percorso di base è il percorso dell’archivio di livello principale che include tutte le risorse che un utente può voler elencare nel componente Ricerca e listener. Se lo desideri, l’utente può anche configurare posizioni specifiche all’interno del percorso di base dalla finestra di dialogo di modifica del componente, in modo che la ricerca venga attivata su posizioni specifiche anziché cercare tutti i nodi all’interno del percorso di base. Per impostazione predefinita, il percorso di base viene utilizzato come criterio del percorso di ricerca per recuperare le risorse, a meno che l’utente non configuri un set di percorsi specifici dall’interno di questa posizione. È importante avere un valore ottimale di questo percorso per fare una ricerca performante. Il valore predefinito del percorso di base rimarrà invariato **_/content/dam/formsanddocuments_** perché tutte le risorse AEM Forms risiedono in **_/content/dam/formsanddocuments._**

Passaggi per configurare il percorso di base

1. Accedi a crx
1. Passa a **/libs/fd/fp/extensions/querybuilder/basepath**

1. Fai clic su &quot;Sovrapponi nodo&quot; nella barra degli strumenti
1. Assicurati che la posizione di sovrapposizione sia &quot;/apps/&quot;
1. Fai clic su Ok
1. Fai clic su Salva
1. Passa alla nuova struttura creata in **/apps/fd/fp/extensions/querybuilder/basepath**

1. Modifica il valore della proprietà path in **&quot;/content/dam&quot;**
1. Fai clic su Salva

Specificando la proprietà del percorso in **&quot;/content/dam&quot;** stai impostando Base Path su /content/dam. Per verificarlo, apri il componente Ricerca e filtro .

![basepath](assets/basepath.png)

## Registrare tipi di risorse personalizzati {#register-custom-asset-types}

È stata aggiunta una nuova scheda (Elenco risorse) nel componente Ricerca e ascoltatore . Questa scheda elenca i tipi di risorse predefiniti e i tipi di risorse aggiuntivi che configuri. Per impostazione predefinita, sono elencati i seguenti tipi di risorse

1. Moduli adattivi
1. Modelli di modulo
1. PDF forms
1. Document(PDF statici)

**Passaggi per registrare il tipo di risorsa personalizzato**

1. Crea un nodo sovrapposto di **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Imposta il percorso di sovrapposizione su &quot;/apps&quot;
1. Passa alla nuova struttura creata in `/apps/fd/fp/extensions/querybuilder/assettypes`

1. In questa posizione, crea un nodo &#39;nt:unstructured&#39; per il tipo da registrare, denomina il nodo **mp4files. Aggiungi le due seguenti proprietà a questo nodo mp4files**

   1. Aggiungi la proprietà jcr:title per specificare il nome visualizzato del tipo di risorsa. Imposta il valore di jcr:title su &quot;File Mp4&quot;.
   1. Aggiungi la proprietà &quot;type&quot; e imposta il suo valore su &quot;videos&quot;. Questo è il valore che utilizziamo nel nostro modello per elencare le risorse dei tipi di video. Salva le modifiche.

1. Crea un nodo di tipo &quot;nt:unstructured&quot; sotto file mp4files. Denomina questo nodo &quot;searchcriteria&quot;
1. Aggiungi uno o più filtri nei criteri di ricerca. Supponiamo che, se l&#39;utente desidera avere un filtro di ricerca per elencare i file mp4Files il cui tipo di MIME è &quot;video/mp4&quot;, è possibile farlo qui
1. Crea un nodo di tipo &quot;nt:unstructured&quot; sotto i criteri di ricerca del nodo. Denomina questo nodo &quot;filetypes&quot;
1. Aggiungi le seguenti 2 proprietà a questo nodo &quot;filetipi&quot;

   1. name: ./jcr:content/metadata/dc:format
   1. valore: video/mp4

1. Ciò significa che le risorse con la proprietà dc:format uguale a video/mp4 sono considerate un tipo di risorsa &quot;Mp4 Videos&quot;. Puoi utilizzare qualsiasi proprietà elencata sul nodo &quot;jcr:content/metadata&quot; per i criteri di ricerca

1. **Assicurati di salvare il tuo lavoro**

Dopo aver eseguito i passaggi precedenti, il nuovo tipo di risorsa (File Mp4) inizierà a essere visualizzato nell’elenco a discesa dei tipi di risorsa del componente Ricerca e Registrazione come mostrato di seguito

![mp4files](assets/mp4files.png)

[In caso di problemi durante il funzionamento, puoi importare il pacchetto seguente.](assets/assettypeskt1.zip) Il pacchetto presenta due tipi di risorse personalizzate definiti. File Mp4 e documenti Worddocuments. Suggerisci di dare un&#39;occhiata al **/apps/fd/fp/extensions/querybuilder/assettypes**

[Installa il pacchetto customeportal](assets/customportalpage.zip). Questo pacchetto contiene la pagina del portale di esempio. Questa pagina viene utilizzata nella parte 2 di questa esercitazione
