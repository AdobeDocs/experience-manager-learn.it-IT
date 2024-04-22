---
title: Autorizzazioni basate sui metadati in AEM Assets
description: Le autorizzazioni basate sui metadati sono una funzione utilizzata per limitare l’accesso in base alle proprietà dei metadati delle risorse, anziché alla struttura delle cartelle.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
thumbnail: xx.jpg
doc-type: Tutorial
source-git-commit: 3b500873ee7307df590ac66dea541a1adf14d726
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# Autorizzazioni basate su metadati{#metadata-driven-permissions}

Le autorizzazioni basate sui metadati sono una funzione utilizzata per consentire alle decisioni di controllo degli accessi sull’istanza di AEM Assets Author di basarsi sulle proprietà dei metadati delle risorse anziché sulla struttura delle cartelle. Con questa funzionalità, puoi definire i criteri di controllo dell’accesso che valutano gli attributi come lo stato delle risorse, il tipo o qualsiasi proprietà di metadati personalizzata definita dall’utente.

Vediamo un esempio. I creativi caricano il loro lavoro in AEM Assets nella cartella correlata alla campagna; potrebbe trattarsi di una risorsa in corso di lavorazione che non è stata approvata per l’uso. Vogliamo assicurarci che gli addetti al marketing visualizzino solo le risorse approvate per questa campagna. Possiamo utilizzare la proprietà dei metadati per indicare che una risorsa è stata approvata e può essere utilizzata dagli esperti di marketing.

## Come funziona

L’abilitazione delle autorizzazioni basate sui metadati implica la definizione delle proprietà dei metadati delle risorse che determineranno le restrizioni di accesso, ad esempio &quot;stato&quot; o &quot;marchio&quot;. Queste proprietà possono quindi essere utilizzate per creare voci di controllo dell’accesso che specificano quali gruppi di utenti hanno accesso alle risorse con valori di proprietà specifici.

## Prerequisiti

Per configurare le autorizzazioni basate sui metadati è necessario accedere a un ambiente AEM as a Cloud Service aggiornato alla versione più recente.


## Passaggi di sviluppo

Per implementare le autorizzazioni basate sui metadati:

1. Determina le proprietà dei metadati della risorsa da utilizzare per il controllo degli accessi. Nel nostro caso sarà una proprietà chiamata `status`.
1. Creare una configurazione OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` nel progetto.
1. Incolla il seguente JSON nel file creato

   ```json
   {
     "restrictionPropertyNames":[
       "status"
     ],
     "restrictionPaths":[
       "/content/dam"
     ]
   }
   ```

1. Sostituisci i nomi delle proprietà e i percorsi di restrizione con i valori richiesti.


Prima di aggiungere voci di controllo dell’accesso basate su restrizioni, è necessario aggiungere una nuova voce di livello superiore per negare prima l’accesso in lettura a tutti i gruppi soggetti alla valutazione delle autorizzazioni per le risorse (ad esempio, &quot;collaboratori&quot; o simili):

1. Passa alla schermata Strumenti → sicurezza → autorizzazioni
1. Selezionare il gruppo &quot;Collaboratori&quot; (o un altro gruppo personalizzato a cui appartengono tutti i gruppi di utenti)
1. Fare clic su &quot;Aggiungi ACE&quot; nell&#39;angolo superiore destro dello schermo
1. Seleziona /content/dam per Percorso
1. Immettete jcr:read per i privilegi
1. Seleziona Rifiuta per tipo di autorizzazione
1. In Restrizioni (Restrictions), selezionate rep:ntNames e immettete dam:Asset come valore di restrizione.
1. Fai clic su Salva

![Nega accesso](./assets/metadata-driven-permissions/deny-access.png)

È ora possibile aggiungere voci di controllo di accesso per concedere l’accesso in lettura ai gruppi di utenti in base ai valori delle proprietà dei metadati delle risorse.

1. Passa alla schermata Strumenti → sicurezza → autorizzazioni
1. Seleziona il gruppo desiderato
1. Fare clic su &quot;Aggiungi ACE&quot; nell&#39;angolo superiore destro dello schermo
1. Seleziona /content/dam (o una sottocartella) per Percorso
1. Immettete jcr:read per i privilegi
1. Seleziona Consenti per tipo di autorizzazione
1. In Restrizioni, seleziona uno dei nomi configurati delle proprietà dei metadati delle risorse (qui verranno incluse le proprietà definite nella configurazione OSGi).
1. Immetti il valore della proprietà metadati richiesta nel campo Valore restrizione
1. Fai clic sull’icona &quot;+&quot; per aggiungere la restrizione alla voce di controllo accesso
1. Fai clic su Salva

![Consenti accesso](./assets/metadata-driven-permissions/allow-access.png)

La cartella di esempio contiene un paio di risorse.

![Visualizzazione amministratore](./assets/metadata-driven-permissions/admin-view.png)

Una volta configurate le autorizzazioni e impostate di conseguenza le proprietà dei metadati della risorsa, gli utenti (nel nostro caso, gli utenti addetti al marketing) vedranno solo la risorsa approvata.

![Visualizzazione addetto marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Vantaggi e considerazioni

I vantaggi delle autorizzazioni basate sui metadati includono:

- Controllo dettagliato sull’accesso alle risorse in base ad attributi specifici.
- Separazione dei criteri di controllo dell’accesso dalla struttura di cartelle, consentendo un’organizzazione più flessibile delle risorse.
- Possibilità di definire regole di controllo di accesso complesse basate su più proprietà di metadati.

>[!NOTE]
>
> È importante notare che:
> 
> - Le proprietà dei metadati vengono valutate in base alle restrizioni utilizzando l’uguaglianza delle stringhe (altri tipi di dati non ancora supportati, ad esempio data)
> - Per consentire più valori per una proprietà di restrizione, è possibile aggiungere ulteriori restrizioni alla voce di controllo dell&#39;accesso selezionando la stessa proprietà dal menu a discesa &quot;Seleziona tipo&quot; e immettendo un nuovo valore di restrizione (ad esempio `status=approved`, `status=wip`) e facendo clic su &quot;+&quot; per aggiungere la restrizione alla voce
> ![Consenti più valori](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Più restrizioni in una singola voce di controllo di accesso con nomi di proprietà diversi (ad esempio `status=approved`, `brand=Adobe`) verrà valutata come una condizione AND, ovvero al gruppo di utenti selezionato verrà concesso l’accesso in lettura alle risorse con `status=approved AND brand=Adobe`
> ![Consenti più restrizioni](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - L&#39;aggiunta di una nuova voce di controllo dell&#39;accesso con una restrizione della proprietà dei metadati determinerà una condizione OR per le voci, ad esempio una singola voce con restrizione `status=approved` e una singola voce con `brand=Adobe` verrà valutato come `status=approved OR brand=Adobe`
> ![Consenti più restrizioni](./assets/metadata-driven-permissions/allow-multiple-aces.png)
