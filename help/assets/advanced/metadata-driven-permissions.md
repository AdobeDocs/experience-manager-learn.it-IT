---
title: Autorizzazioni basate sui metadati in AEM Assets
description: Le autorizzazioni basate sui metadati sono una funzione utilizzata per limitare l’accesso in base alle proprietà dei metadati delle risorse, anziché alla struttura delle cartelle.
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
duration: 158
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 0%

---

# Autorizzazioni basate su metadati{#metadata-driven-permissions}

Le autorizzazioni basate sui metadati sono una funzione utilizzata per consentire alle decisioni di controllo degli accessi sull’istanza di AEM Assets Author di basarsi sul contenuto delle risorse o sulle proprietà dei metadati anziché sulla struttura delle cartelle. Con questa funzionalità, puoi definire i criteri di controllo dell’accesso che valutano attributi quali lo stato delle risorse, il tipo o qualsiasi proprietà personalizzata definita.

Vediamo un esempio. I creativi caricano il loro lavoro in AEM Assets nella cartella correlata alla campagna; potrebbe trattarsi di una risorsa in corso di lavorazione che non è stata approvata per l’uso. Vogliamo assicurarci che gli addetti al marketing visualizzino solo le risorse approvate per questa campagna. Possiamo utilizzare una proprietà di metadati per indicare che una risorsa è stata approvata e può essere utilizzata dagli esperti di marketing.

## Come funziona

L’abilitazione delle autorizzazioni basate sui metadati implica la definizione del contenuto della risorsa o delle proprietà dei metadati che determineranno le restrizioni di accesso, ad esempio &quot;stato&quot; o &quot;marchio&quot;. Queste proprietà possono quindi essere utilizzate per creare voci di controllo dell’accesso che specificano quali gruppi di utenti hanno accesso alle risorse con valori di proprietà specifici.

## Prerequisiti

Per configurare le autorizzazioni basate sui metadati è necessario accedere a un ambiente AEM as a Cloud Service aggiornato alla versione più recente.

## Configurazione OSGi {#configure-permissionable-properties}

Per implementare le autorizzazioni basate sui metadati, uno sviluppatore deve implementare in AEM as a Cloud Service una configurazione OSGi che consenta a contenuti specifici di risorse o proprietà di metadati di abilitare le autorizzazioni basate sui metadati.

1. Determina il contenuto della risorsa o le proprietà dei metadati che verranno utilizzati per il controllo degli accessi. I nomi delle proprietà sono i nomi delle proprietà JCR nella risorsa `jcr:content` o `jcr:content/metadata` della risorsa. Nel nostro caso sarà una proprietà denominata `status`.
1. Crea una configurazione OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` nel progetto AEM Maven.
1. Incolla il seguente JSON nel file creato:

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "restrictionContentPropertyNames":[],
     "enabled":true
   }
   ```

1. Sostituisci i nomi delle proprietà con i valori richiesti.  La proprietà di configurazione `restrictionContentPropertyNames` viene utilizzata per abilitare le autorizzazioni per le proprietà della risorsa `jcr:content`, mentre la proprietà di configurazione `restrictionPropertyNames` abilita le autorizzazioni per le proprietà della risorsa `jcr:content/metadata` per le risorse.

## Reimposta autorizzazioni risorsa base

Prima di aggiungere voci di controllo dell’accesso basate su restrizioni, è necessario aggiungere una nuova voce di livello superiore per negare prima l’accesso in lettura a tutti i gruppi soggetti alla valutazione delle autorizzazioni per Assets (ad esempio, &quot;collaboratori&quot; o simili):

1. Passa alla schermata __Strumenti → Autorizzazioni → di sicurezza__
1. Seleziona il gruppo __Collaboratori__ (o un altro gruppo personalizzato a cui appartengono tutti i gruppi di utenti)
1. Fai clic su __Aggiungi ACE__ nell&#39;angolo superiore destro della schermata
1. Seleziona `/content/dam` per __Percorso__
1. Immetti `jcr:read` per __Privilegi__
1. Seleziona `Deny` per __Tipo di autorizzazione__
1. In Restrizioni, selezionare `rep:ntNames` e immettere `dam:Asset` come __Valore restrizione__
1. Fai clic su __Salva__

![Nega accesso](./assets/metadata-driven-permissions/deny-access.png)

## Concedere l’accesso alle risorse tramite metadati

È ora possibile aggiungere voci di controllo di accesso per concedere l&#39;accesso in lettura ai gruppi di utenti in base ai [valori configurati per la proprietà dei metadati delle risorse](#configure-permissionable-properties).

1. Passa alla schermata __Strumenti → Autorizzazioni → di sicurezza__
1. Seleziona i gruppi di utenti che devono avere accesso alle risorse
1. Fai clic su __Aggiungi ACE__ nell&#39;angolo superiore destro della schermata
1. Seleziona `/content/dam` (o una sottocartella) per __Percorso__
1. Immetti `jcr:read` per __Privilegi__
1. Seleziona `Allow` per __Tipo di autorizzazione__
1. In __Restrizioni__, seleziona uno dei [nomi di proprietà dei metadati delle risorse configurati nella configurazione OSGi](#configure-permissionable-properties)
1. Immetti il valore della proprietà metadati richiesta nel campo __Valore restrizione__
1. Fai clic sull&#39;icona __+__ per aggiungere la restrizione alla voce di controllo di accesso
1. Fai clic su __Salva__

![Consenti accesso](./assets/metadata-driven-permissions/allow-access.png)

## Autorizzazioni basate sui metadati attive

La cartella di esempio contiene un paio di risorse.

![Visualizzazione amministratore](./assets/metadata-driven-permissions/admin-view.png)

Dopo aver configurato le autorizzazioni e impostato di conseguenza le proprietà dei metadati della risorsa, gli utenti (l’utente addetto al marketing nel nostro caso) visualizzeranno solo la risorsa approvata.

![Visualizzazione addetto al marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Vantaggi e considerazioni

I vantaggi delle autorizzazioni basate sui metadati includono:

- Controllo dettagliato sull’accesso alle risorse in base ad attributi specifici.
- Separazione dei criteri di controllo dell’accesso dalla struttura di cartelle, consentendo un’organizzazione più flessibile delle risorse.
- Possibilità di definire regole di controllo di accesso complesse basate su più contenuti o proprietà di metadati.

>[!NOTE]
>
> È importante notare che:
> 
> - Le proprietà vengono valutate in base alle restrizioni utilizzando __Uguaglianza stringa__ (`=`) (altri tipi di dati o operatori non sono ancora supportati, per valori maggiori di (`>`) o proprietà Data)
> - Per consentire più valori per una proprietà di restrizione, è possibile aggiungere ulteriori restrizioni alla voce di controllo dell&#39;accesso selezionando la stessa proprietà dal menu a discesa &quot;Seleziona tipo&quot; e immettendo un nuovo valore di restrizione (ad esempio `status=approved`, `status=wip`) e facendo clic su &quot;+&quot; per aggiungere la restrizione alla voce
> ![Consenti più valori](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Sono supportate __restrizioni AND__, tramite più restrizioni in una singola voce di controllo di accesso con nomi di proprietà diversi (ad esempio `status=approved`, `brand=Adobe`) verrà valutata come condizione AND, ovvero al gruppo di utenti selezionato verrà concesso l&#39;accesso in lettura alle risorse con `status=approved AND brand=Adobe`
> ![Consenti più restrizioni](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - Le __restrizioni OR__ sono supportate aggiungendo una nuova voce di controllo di accesso con una restrizione di proprietà dei metadati che stabilirà una condizione OR per le voci. Ad esempio, una singola voce con restrizione `status=approved` e una singola voce con `brand=Adobe` verranno valutate come `status=approved OR brand=Adobe`
> ![Consenti più restrizioni](./assets/metadata-driven-permissions/allow-multiple-aces.png)
