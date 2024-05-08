---
title: Autorizzazioni basate sui metadati in AEM Assets
description: Le autorizzazioni basate sui metadati sono una funzione utilizzata per limitare l’accesso in base alle proprietà dei metadati delle risorse, anziché alla struttura delle cartelle.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 98b26eb15c2fe7d1cf73fe028b2db24087c813a5
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 0%

---

# Autorizzazioni basate su metadati{#metadata-driven-permissions}

Le autorizzazioni basate sui metadati sono una funzione utilizzata per consentire alle decisioni di controllo degli accessi sull’istanza di AEM Assets Author di basarsi sulle proprietà dei metadati delle risorse anziché sulla struttura delle cartelle. Con questa funzionalità, puoi definire i criteri di controllo dell’accesso che valutano gli attributi come lo stato delle risorse, il tipo o qualsiasi proprietà di metadati personalizzata definita dall’utente.

Vediamo un esempio. I creativi caricano il loro lavoro in AEM Assets nella cartella correlata alla campagna; potrebbe trattarsi di una risorsa in corso di lavorazione che non è stata approvata per l’uso. Vogliamo assicurarci che gli addetti al marketing visualizzino solo le risorse approvate per questa campagna. Possiamo utilizzare la proprietà dei metadati per indicare che una risorsa è stata approvata e può essere utilizzata dagli esperti di marketing.

## Come funziona

L’abilitazione delle autorizzazioni basate sui metadati implica la definizione delle proprietà dei metadati delle risorse che determineranno le restrizioni di accesso, ad esempio &quot;stato&quot; o &quot;marchio&quot;. Queste proprietà possono quindi essere utilizzate per creare voci di controllo dell’accesso che specificano quali gruppi di utenti hanno accesso alle risorse con valori di proprietà specifici.

## Prerequisiti

Per configurare le autorizzazioni basate sui metadati è necessario accedere a un ambiente AEM as a Cloud Service aggiornato alla versione più recente.

## Configurazione OSGi {#configure-permissionable-properties}

Per implementare le autorizzazioni basate sui metadati, uno sviluppatore deve implementare una configurazione OSGi in AEM as a Cloud Service, che consenta a specifiche proprietà di metadati delle risorse di abilitare le autorizzazioni basate sui metadati.

1. Determina le proprietà dei metadati della risorsa da utilizzare per il controllo degli accessi. I nomi delle proprietà sono i nomi delle proprietà JCR sul `jcr:content/metadata` risorsa. Nel nostro caso sarà una proprietà chiamata `status`.
1. Creare una configurazione OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` nel progetto AEM Maven.
1. Incolla il seguente JSON nel file creato:

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. Sostituisci i nomi delle proprietà con i valori richiesti.

## Reimposta autorizzazioni risorsa base

Prima di aggiungere voci di controllo dell’accesso basate su restrizioni, è necessario aggiungere una nuova voce di livello superiore per negare prima l’accesso in lettura a tutti i gruppi soggetti alla valutazione delle autorizzazioni per le risorse (ad esempio, &quot;collaboratori&quot; o simili):

1. Accedi a __Strumenti → Autorizzazioni → di sicurezza__ screen
1. Seleziona la __Collaboratori__ gruppo (o altro gruppo personalizzato a cui appartengono tutti i gruppi di utenti)
1. Clic __Aggiungi ACE__ nell&#39;angolo superiore destro dello schermo
1. Seleziona `/content/dam` per __Percorso__
1. Invio `jcr:read` per __Privilegi__
1. Seleziona `Deny` per __Tipo di autorizzazione__
1. In Restrizioni, seleziona `rep:ntNames` e immetti `dam:Asset` come __Valore di restrizione__
1. Clic __Salva__

![Nega accesso](./assets/metadata-driven-permissions/deny-access.png)

## Concedere l’accesso alle risorse tramite metadati

È ora possibile aggiungere voci di controllo dell’accesso per concedere l’accesso in lettura ai gruppi di utenti in base al [valori delle proprietà dei metadati della risorsa configurati](#configure-permissionable-properties).

1. Accedi a __Strumenti → Autorizzazioni → di sicurezza__ screen
1. Seleziona i gruppi di utenti che devono avere accesso alle risorse
1. Clic __Aggiungi ACE__ nell&#39;angolo superiore destro dello schermo
1. Seleziona `/content/dam` (o una sottocartella) per __Percorso__
1. Invio `jcr:read` per __Privilegi__
1. Seleziona `Allow` per __Tipo di autorizzazione__
1. Sotto __Restrizioni__, seleziona una delle opzioni [nomi di proprietà dei metadati della risorsa configurati nella configurazione OSGi](#configure-permissionable-properties)
1. Immetti il valore della proprietà dei metadati richiesta nella __Valore di restrizione__ campo
1. Fai clic su __+__ per aggiungere la limitazione alla voce di controllo accesso
1. Clic __Salva__

![Consenti accesso](./assets/metadata-driven-permissions/allow-access.png)

## Autorizzazioni basate sui metadati attive

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
> - Le proprietà dei metadati vengono valutate in base alle restrizioni utilizzando __Uguaglianza stringa__ (`=`) (altri tipi di dati o operatori non sono ancora supportati, per un valore maggiore di (`>`) o proprietà Date)
> - Per consentire più valori per una proprietà di restrizione, è possibile aggiungere ulteriori restrizioni alla voce di controllo dell&#39;accesso selezionando la stessa proprietà dal menu a discesa &quot;Seleziona tipo&quot; e immettendo un nuovo valore di restrizione (ad esempio `status=approved`, `status=wip`) e facendo clic su &quot;+&quot; per aggiungere la restrizione alla voce
> ![Consenti più valori](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __Restrizioni AND__ sono supportate tramite più restrizioni in una singola voce di controllo dell’accesso con nomi di proprietà diversi (ad esempio `status=approved`, `brand=Adobe`) verrà valutata come una condizione AND, ovvero al gruppo di utenti selezionato verrà concesso l’accesso in lettura alle risorse con `status=approved AND brand=Adobe`
> ![Consenti più restrizioni](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __Limitazioni per le operazioni OR__ sono supportati aggiungendo una nuova voce di controllo dell’accesso con una restrizione della proprietà dei metadati che stabilirà una condizione OR per le voci, ad esempio una singola voce con restrizione `status=approved` e una singola voce con `brand=Adobe` verrà valutato come `status=approved OR brand=Adobe`
> ![Consenti più restrizioni](./assets/metadata-driven-permissions/allow-multiple-aces.png)
