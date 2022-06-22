---
title: Modelli e archetipi di governance e personale
description: Scopri come utilizzare la piattaforma Adobe Experience Manager (AEM) e ottenere il massimo dai tuoi sforzi.
solution: Experience Manager
exl-id: 808ab7a6-5ec5-4bbd-9a6e-cfc0b447430d
source-git-commit: 89982f506a5e1ffc12f84a0f616aaa1dc2e00c5b
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 0%

---

# Adobe Experience Manager (AEM) - Modelli e archetipi per la governance e il personale

In qualità di leader dell&#39;esperienza del cliente, Adobe capisce quanto sia difficile per te assicurarsi di disporre delle persone e del framework di governance giusti per incrementare l&#39;efficienza operativa. Con la governance e i modelli di personale collaudati nel settore Adobe, hai gli strumenti e le conoscenze per creare una solida base di gestione dei contenuti e delle risorse. In questo articolo discuteremo di come rendere operativa la tua piattaforma Adobe Experience Manager (AEM) e ottenere il massimo valore dai tuoi sforzi.

## Creare un framework operativo superiore

Per essere in grado di eseguire e operare AEM considerare i seguenti elementi:

* Esegui milestone strategiche - Ci saranno molte tappe strategiche, (personalizzazione, integrazione multicanale, ecc.) che non può essere eseguito a meno che non sia stato implementato il modello di assegnazione del personale corretto.
* Creare una base per la trasformazione digitale - AEM viene spesso utilizzato come primo passo del processo di modernizzazione di un&#39;organizzazione. L&#39;impostazione di una base ti consente di sfruttare AEM a pieno la sua capacità.
* Coinvolgimento dell’utente: disporre di un team per eseguire il lavoro tattico (aggiornare flussi di lavoro, autorizzazioni, CSS, ecc.) Più ci sono differenze tra ciò che gli utenti vogliono e ciò che vengono dati, più sono frustrati possono diventare. È importante mantenere gli utenti investiti nel sistema, investiti nella soluzione e disporre del modello operativo corretto.

Qual è quindi il modello giusto? Qual è la giusta matrice di ruoli da creare?

Non esiste una sola risposta specifica perché, così come le organizzazioni variano notevolmente, anche una configurazione AEM può variare notevolmente, il che rende necessaria la presenza di ruoli di supporto diversi. Ogni verticale, ogni settore, ogni struttura del team richiederà un&#39;implementazione diversa. Ma si può creare una linea di base stabilendo archetipi.

## Archetipi

Gli archetipi sono idee di ruolo specifiche, di alto livello, mappate su attributi specifici. Questo a sua volta può essere utilizzato per creare una premessa fondamentale che aiuta a informare il modello di cui hai veramente bisogno. È importante notare che gli archetipi non sono limitati a una persona per archetipo. Ad esempio, un bibliotecario DAM potrebbe avere qualche esperienza tecnica.

### Flussi di operazionalizzazione

Esistono due flussi di operazionalizzazione per [!DNL AEM Sites] e [!DNL AEM Assets]:

1. Operazioni quotidiane di base (aggiornamento metadati)

1. Strategia e lavoro di trasformazione, come i grandi progetti interorganizzativi

![flussi di operazionalizzazione](assets/streams-of-operationalization.png)

### Ruoli di risorse AEM di alto livello

**Differenza generale:** Questa linea di base supporta modelli centralizzati e decentrati. Se si dispone di un modello decentralizzato, AEM può essere utilizzato in modo astratto. Il ruolo Proprietario prodotto deve essere utilizzato in modo creativo, ma è necessario disporre anche di un Proprietario prodotto che sia proprietario dei diversi stili per un tipo di risorsa e di un altro che sovrintenda all’intera organizzazione.

1. Ruoli di esecuzione e funzionamento di base

   * Risorsa tecnica : chi ha AEM esperienza capisce le autorizzazioni e può aggiornare lo schema dei metadati
   * Release manager
   * Proprietario del prodotto: ruolo allineato alla soluzione. Alcuni proprietari di prodotti possono essere coinvolti in analytics.
   * Bibliotecario DAM - Questo è qualcuno che può aiutare a gestire i processi di framework integrativo. Questo ruolo creativo può sovrapporsi ad altri ruoli. (Nota: questo è un ruolo che è esploso in popolarità negli ultimi cinque anni).
   * Contenuto creativo

1. Strategia e trasformazione

   * Team di sviluppo: questo team è necessario per l&#39;utilizzo di un&#39;importante pietra miliare strategica.
   * Business Architect: sviluppa i requisiti per supportare le fasi tecniche e le iniziative strategiche; può essere compensato con un proprietario di prodotto aggiuntivo
   * Architetto tecnico : un utente che ha una comprensione a livello aziendale e una presenza costante in tutta l’organizzazione. Questo ruolo funge da punto centrale della verità DAM.

**Scenari di esempio**

1. **Esegui e funziona:**

Di seguito sono riportati alcuni esempi di casi di utilizzo per uno scenario leggero (società di abbigliamento sportivo) e pesante (società cosmetica):

1. Leggero - ruoli della società di abbigliamento sportivo:

   * Sviluppatori a tempo parziale, offshore
   * 1 proprietario del prodotto - Tempo pieno, onshore
   * 1 DAM Library - Tempo pieno, onshore
   * 1 Architetto Tecnico - Tempo parziale, onshore
   * 1 Release Manager - part time, onshore

1. Pesante - Azienda cosmetica (Multi-Brand)

   * 3 sviluppatori a tempo pieno - a tempo pieno, offshore
   * 4 Proprietari di prodotto - 3 marchi specifici, 1 primario
   * 1 DAM Library - Tempo pieno, onshore
   * 4 amministratori principali PMI per marchio
   * 1 Architetto tecnico

### Alta [!DNL AEM Sites] ruoli

1. Funzionamento di base

   **Differenza generale:** Gli sviluppatori CSS creano nuove interfacce per i componenti. L&#39;Adobe di Sr Business Consultant, Joseph Van Buskirk, raccomanda di &quot;Ottenere componenti e sistemi di stile non collegati. Questo è il ruolo che guida il risparmio sui costi. L’80% delle esperienze create deve essere fatto utilizzando i componenti core o creati in precedenza.&quot; L’obiettivo è ridefinire i componenti core o personalizzati con nuovi stili utilizzando uno sviluppatore CSS (o un team di sviluppo front-end) .

   Esempi di ruolo:

   * Sviluppo CSS: crea artefatti di esperienza riutilizzando i componenti con nuovi stili.
   * Sviluppo back-end : crea nuovi componenti o può estendere un componente core. Se eseguito correttamente, questo ruolo non deve avere più di una persona, a meno che non sia necessario eseguire grandi attività di animazione.
   * Gestione delle versioni: controlla la distribuzione del codice e funge da tecnico del successo cliente corrente.
   * Proprietario del prodotto - collabora con BU sul matrimonio Visioni tecniche e strategiche; crea attività di manutenzione e miglioramenti e funge da proprietario aziendale della soluzione.
   * Autori amministratori : aggiorna lo skin CSS e fornisce indicazioni agli autori che aggiornano e applicano contenuti. Questo ruolo funziona sulle configurazioni del flusso di lavoro e crea una documentazione guida per gli autori dei contenuti da applicare. NOTA: Nella versione 6.5, Adobe consiglia di utilizzare modelli modificabili.
   * Autori dei contenuti : applica contenuti, acquisisce una proprietà su più livelli e fornisce problemi e preoccupazioni di comunicazione man mano che si verificano con CSM.

1. Strategia e trasformazione

   Esempi di ruolo:

   * Team di sviluppo: fornisce conoscenze AEM ed esegue nuove fasi di trasformazione con l&#39;architetto tecnico.
   * Architetto tecnico - fornisce conoscenze sull&#39;integrazione, lavora con il proprietario del prodotto per mappare le tappe tecniche e fornisce una profonda conoscenza tecnica di AEM.
   * Architetto aziendale: consente di creare attività per le storie utente e di gestire le tappe tecniche e aziendali del proprietario del prodotto.

### Scenari di esempio

Di seguito sono riportati alcuni esempi di ruolo per uno scenario client leggero e pesante:

1. Chiaro

   * 2 Sviluppatori CSS - onshore
   * 1 proprietario del prodotto - a tempo pieno, onshore
   * 1 Sviluppatore back-end - offshore
   * 1 Architetto tecnico - onshore
   * 1 Release manager - part time, onshore

1. Pesante (incentrato sulla campagna)

   * 4 Sviluppatori CSS - a tempo pieno, onshore
   * 2 Sviluppatori back-end - a tempo pieno, on-shore
   * 1 Architetto tecnico - onshore
   * 1 proprietario del prodotto
   * 2 Architetti aziendali - offshore

### Aree principali

**Comprendere gli archetipi** — Iniziare lentamente, comprendere e analizzare gli archetipi. Siate creativi e flessibili, tenendo presente che non esiste un modello corretto da seguire.

**Comprendere la roadmap** - Alcune organizzazioni hanno molte tappe che desiderano eseguire. Preparati ad allocare più risorse tecniche di quanto si possa stimare.

**Utilizzo delle risorse interne** - I gap possono venire inaspettatamente. Puoi riempirli più rapidamente acquistando membri del team interno, invece di cercare all’esterno dell’organizzazione.

Per una discussione più approfondita su Modelli e Archetipi di Governance e Staffing, ascoltate questa discussione di un&#39;ora: [Archetipi di ruolo e creazione di un quadro operativo per [!DNL AEM Assets] e [!DNL Sites]](https://adobecustomersuccess.adobeconnect.com/p8ml5nmy0758mp4/)
