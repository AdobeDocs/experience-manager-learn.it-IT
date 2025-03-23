---
title: Contenuto protetto in AEM Headless
description: Scopri come proteggere i contenuti in AEM Headless.
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-05-01T00:00:00Z
exl-id: c4b093d4-39b8-4f0b-b759-ecfbb6e9e54f
duration: 254
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 0%

---

# Protezione dei contenuti in AEM Headless

Assicurare l’integrità e la sicurezza dei dati durante la distribuzione di contenuti AEM headless da AEM Publish è fondamentale per la distribuzione di contenuti sensibili. Questo tutorial illustra come proteggere il contenuto gestito dagli endpoint API GraphQL headless di AEM.

Le indicazioni contenute in questo tutorial in caso di requisiti rigorosi per la disponibilità esclusiva di contenuti per utenti o gruppi di utenti specifici. È fondamentale distinguere tra contenuti di marketing personalizzati e contenuti privati, come PII o dati finanziari personali, per evitare confusione e risultati indesiderati. Questo tutorial tratta la protezione dei contenuti privati.

Quando si parla di contenuti di marketing, si fa riferimento a contenuti personalizzati per singoli utenti o gruppi, che non sono destinati al consumo generico. Tuttavia, è essenziale comprendere che, sebbene questo contenuto possa essere destinato a determinati utenti, la sua esposizione al di fuori del contesto previsto (ad esempio, attraverso la manipolazione delle richieste HTTP) non pone un rischio legale, di sicurezza o di reputazione.

Si sottolinea che tutti i contenuti trattati in questo articolo sono considerati privati e possono essere visualizzati solo da utenti o gruppi designati. I contenuti di marketing spesso non richiedono protezione, ma la loro distribuzione a utenti specifici può essere gestita dall’applicazione e memorizzata nella cache per migliorare le prestazioni.

Questo manuale non tratta:

- Proteggere direttamente gli endpoint, ma concentrarsi sulla protezione del contenuto che distribuiscono.
- Autenticazione per pubblicazione AEM o recupero dei token di accesso. I metodi di autenticazione e il passaggio delle credenziali dipendono dai singoli casi d’uso e dalle implementazioni.

## Gruppi di utenti

Innanzitutto, è necessario definire un [gruppo utenti](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) contenente gli utenti che devono avere accesso al contenuto protetto.

![Gruppo di utenti con contenuto protetto AEM Headless](./assets/protected-content/user-groups.png){align="center"}

I gruppi di utenti assegnano l’accesso a contenuti AEM headless, inclusi frammenti di contenuto o altre risorse di riferimento.

1. Accedi ad AEM Author come **amministratore utenti**.
1. Passa a **Strumenti** > **Sicurezza** > **Gruppi**.
1. Seleziona **Crea** nell&#39;angolo superiore destro.
1. Nella scheda **Dettagli**, specifica **ID gruppo** e **Nome gruppo**.
   - L&#39;ID gruppo e il Nome gruppo possono essere qualsiasi cosa, ma in questo esempio utilizza il nome **Utenti API AEM headless**.
1. Seleziona **Salva e chiudi**.
1. Seleziona il gruppo appena creato, quindi scegli **Attiva** dalla barra delle azioni.

Se sono necessari vari livelli di accesso, crea più gruppi di utenti che possono essere associati a contenuti diversi.

### Aggiunta di utenti ai gruppi di utenti

Per consentire alle richieste API di AEM Headless GraphQL di accedere al contenuto protetto, puoi associare la richiesta headless a un utente appartenente a un gruppo di utenti specifico. Di seguito sono riportati due approcci comuni:

1. **AEM as a Cloud Service [account tecnici](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - Crea un account tecnico in AEM as a Cloud Service Developer Console.
   - Accedi ad AEM Author una volta con l’account tecnico.
   - Aggiungi l&#39;account tecnico al gruppo di utenti tramite **Strumenti > Sicurezza > Gruppi > Utenti API headless AEM > Membri**.
   - **Attiva** sia l&#39;utente dell&#39;account tecnico che il gruppo di utenti in AEM Publish.
   - Questo metodo richiede che il client headless non esponga le credenziali del servizio all’utente, in quanto sono credenziali per un utente specifico e non devono essere condivise.

   ![Gestione dei gruppi di account tecnici AEM](./assets/protected-content/group-membership.png){align="center"}

2. **Utenti con nome:**
   - Autentica gli utenti denominati e aggiungili direttamente al gruppo di utenti in AEM Publish.
   - Questo metodo richiede che il client headless autentichi le credenziali utente con AEM Publish, ottenga un token di accesso o AEM e utilizzi questo token per le richieste successive ad AEM. I dettagli su come ottenere questo risultato non sono descritti in questa procedura e dipendono dall’implementazione.

## Protezione dei frammenti di contenuto

La protezione dei frammenti di contenuto è essenziale per proteggere i contenuti AEM headless e si ottiene associando i contenuti a un gruppo utenti chiuso (CUG). Quando un utente effettua una richiesta all’API GraphQL headless di AEM, il contenuto restituito viene filtrato in base ai CUG dell’utente.

![CUG AEM headless](./assets/protected-content/cugs.png){align="center"}

Segui questi passaggi per ottenere questo risultato tramite [Gruppi utenti chiusi (CUG)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups).

1. Accedi ad AEM Author come **utente DAM**.
2. Passa a **Assets > File** e seleziona la **cartella** contenente i frammenti di contenuto da proteggere. I CUG vengono applicati in modo gerarchico ed hanno effetto sulle sottocartelle, a meno che non siano sostituiti da un CUG diverso.
   - Assicurati che gli utenti appartenenti ad altri canali che utilizzano il contenuto delle cartelle siano inclusi in questo gruppo di utenti. In alternativa, includi i gruppi di utenti associati a tali canali nell’elenco dei CUG. In caso contrario, il contenuto non sarà accessibile a tali canali.
3. Selezionare la cartella e scegliere **Proprietà** dalla barra degli strumenti.
4. Selezionare la scheda **Autorizzazioni**.
5. Digita il **Nome gruppo** e seleziona il pulsante **Aggiungi** per aggiungere il nuovo CUG.
6. **Salva** per applicare il gruppo utenti chiusi (CUG).
7. **Seleziona** la cartella delle risorse e seleziona **Pubblica** per inviare la cartella con i gruppi utenti chiusi applicati ad AEM Publish, dove verrà valutata come autorizzazione.

Segui gli stessi passaggi per tutte le cartelle contenenti frammenti di contenuto da proteggere, applicando i CUG corretti a ciascuna cartella.

Ora, quando viene effettuata una richiesta HTTP all’endpoint API GraphQL headless di AEM, nel risultato verranno inclusi solo i Frammenti di contenuto accessibili dai CUG specificati dell’utente richiedente. Se l’utente non ha accesso a nessun frammento di contenuto, il risultato sarà vuoto, ma verrà comunque restituito il codice di stato HTTP 200.

### Protezione del contenuto di riferimento

I frammenti di contenuto spesso fanno riferimento ad altri contenuti AEM come le immagini. Per proteggere il contenuto a cui si fa riferimento, applica i CUG alle cartelle di risorse in cui sono memorizzate le risorse a cui si fa riferimento. Tieni presente che le risorse a cui si fa riferimento vengono comunemente richieste utilizzando metodi diversi da quelli delle API GraphQL headless di AEM. Di conseguenza, il modo in cui i token di accesso vengono trasmessi alle richieste di tali risorse a cui si fa riferimento può variare.

A seconda dell’architettura dei contenuti, potrebbe essere necessario applicare gruppi utenti chiusi (CUG) a più cartelle per garantire la protezione di tutti i contenuti di riferimento.

## Impedisci la memorizzazione nella cache di contenuto protetto

AEM as a Cloud Service [memorizza nella cache le risposte HTTP per impostazione predefinita](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) per migliorare le prestazioni. Tuttavia, questo può causare problemi nella distribuzione dei contenuti protetti. Per impedire la memorizzazione nella cache di tali contenuti, [rimuovi le intestazioni della cache per endpoint specifici](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) nella configurazione Apache dell&#39;istanza AEM Publish.

Aggiungi la seguente regola al file di configurazione Apache del progetto Dispatcher per rimuovere le intestazioni cache per endpoint specifici:

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

Tieni presente che questo comporterà una riduzione delle prestazioni in quanto il contenuto non verrà memorizzato nella cache dal dispatcher o dalla CDN. Si tratta di un compromesso tra prestazioni e sicurezza.

## Protezione degli endpoint API di AEM Headless GraphQL

Questa guida non tratta la protezione degli stessi [endpoint API AEM Headless GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint), ma si concentra sulla protezione del contenuto da essi fornito. Tutti gli utenti, inclusi gli utenti anonimi, possono accedere agli endpoint contenenti contenuto protetto. Verrà restituito solo il contenuto accessibile dai gruppi utenti chiusi dell’utente. Se nessun contenuto è accessibile, la risposta API headless di AEM avrà comunque un codice di stato di risposta HTTP 200, ma i risultati saranno vuoti. In genere, la protezione del contenuto è sufficiente, in quanto gli endpoint stessi non espongono intrinsecamente dati sensibili. Se devi proteggere gli endpoint, applica gli ACL su AEM Publish tramite [script di inizializzazione archivio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
