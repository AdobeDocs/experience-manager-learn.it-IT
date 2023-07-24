---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Integrare Adobe Analytics con Customer Journey Analytics

{{analytics-description}}

{{customer-journey-analytics-description}}

L’integrazione di Adobe Analytics con Customer Journey Analytics offre i seguenti vantaggi:

+ **Informazioni complete** nei comportamenti e nelle preferenze del cliente.
+ **Tracciamento cross-channel perfetto** per una visione olistica.
+ **Dati e reporting unificati** per analisi accurate.
+ **Personalizzazione avanzata** e un maggiore coinvolgimento dei clienti.
+ **Informazioni sui dati in tempo reale** agile processo decisionale.

## Integrazioni comuni

<table>
    <thead>
        <tr>
            <td>applicazioni Experience Cloud</td>
            <td>Integra tramite</td>
            <td>Quando utilizzare</td>
            <td>Casi d’uso comuni</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics con Customer Journey Analytics</a></td>
            <td>Connettore sorgente in Experience Platform</td>
            <td>
                <ul>
                    <li>TODO: utilizza questa integrazione per acquisire i dati di Analytics in Experience Platform dalle suite di rapporti.</li>
                    <li>Quando la disponibilità dei dati per il profilo cliente può essere compresa tra 2 e 30 minuti dal momento della raccolta dei dati e la disponibilità per il Data Lake può arrivare a 90 minuti.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Flusso di lavoro semplice e avviato dall'interfaccia utente.</li>
                    <li>Mappatura dell’interfaccia utente per copiare le proprietà e le eVar di Analytics in nuovi campi XDM.</li>
                    <li>Il modo più rapido per ottenere valore dal profilo cliente in tempo reale e dal Customer Journey Analytics.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">COLLEGA A: Analytics con Customer Journey Analytics</a></td>
            <td>Bordo Experience Platform</td>
            <td>
                <ul>
                    <li>Quando desideri implementare una strategia a lungo termine. In questo modo i dati vengono inviati direttamente da un dispositivo all’Experience Platform utilizzando AEP Web SDK, AEP Mobile SDK o l’API del server di rete Edge.</li>
                    <li>Se sei un nuovo cliente o un cliente esistente, che necessita della disponibilità dei dati di Analytics per il profilo cliente per supportare casi d’uso per la personalizzazione della stessa pagina e di quella successiva.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Fornisce il massimo livello di controllo per i dati raccolti da utilizzare per supportare i casi d’uso.</li>
                    <li>I dati lato client vengono facilmente mappati sui campi XDM.</li>
                    <li>Disponibilità dei dati più rapida per Real-Time Customer Profile.</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>
