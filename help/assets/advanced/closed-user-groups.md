---
title: Gruppi di utenti chiusi in AEM Assets
description: I gruppi di utenti chiusi (CUG) sono una funzione utilizzata per limitare l’accesso al contenuto a un gruppo selezionato di utenti su un sito pubblicato. Questo video mostra come i gruppi di utenti chiusi possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse.
version: 6.3, 6.4, 6.5, cloud-service
topic: Amministrazione, sicurezza
feature: Utenti e gruppi
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---


# Gruppi di utenti chiusi{#using-closed-user-groups-with-aem-assets}

I gruppi di utenti chiusi (CUG) sono una funzione utilizzata per limitare l’accesso al contenuto a un gruppo selezionato di utenti su un sito pubblicato. Questo video mostra come i gruppi di utenti chiusi possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse. Il supporto per i gruppi di utenti chiusi con AEM Assets è stato introdotto per la prima volta in AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Gruppo utenti chiuso (CUG) con AEM Assets

* Progettato per limitare l’accesso alle risorse in un’istanza di AEM Publish.
* Consente l&#39;accesso in lettura a un set di utenti/gruppi.
* Il gruppo utenti chiuso può essere configurato solo a livello di cartella. Impossibile impostare CUG su singole risorse.
* I criteri CUG vengono ereditati automaticamente da qualsiasi sottocartella e risorsa applicata.
* I criteri CUG possono essere ignorati dalle sottocartelle impostando un nuovo criterio CUG. Questo dovrebbe essere utilizzato con moderazione e non è considerato una best practice.

## Gruppi di utenti chiusi e elenchi di controllo degli accessi {#closed-user-groups-vs-access-control-lists}

I gruppi di utenti chiusi (CUG) e gli elenchi di controllo accessi (ACL) vengono utilizzati per controllare l’accesso al contenuto in AEM e in base AEM utenti e gruppi di sicurezza. Tuttavia, l’applicazione e l’implementazione di queste funzioni sono molto diverse. Nella tabella seguente sono riepilogate le distinzioni tra le due feature.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configura e applica le autorizzazioni per il contenuto dell&#39;istanza AEM **corrente**. | Configura i criteri CUG per i contenuti nell&#39;istanza AEM **author** . Applica i criteri CUG per i contenuti nelle istanze AEM **publish** . |
| Livelli di autorizzazione | Definisce le autorizzazioni concesse/negate per utenti/gruppi per tutti i livelli: Leggi, Modifica, Crea, Elimina, Leggi ACL, Modifica ACL, Replicare. | Consente l&#39;accesso in lettura a un set di utenti/gruppi. Nega l&#39;accesso in lettura a *tutti gli altri utenti/gruppi*. |
| Pubblicazione | Le ACL sono *non* pubblicate con il contenuto. | I criteri CUG *sono* pubblicati con il contenuto. |

## Collegamenti di supporto {#supporting-links}

* [Gestione delle risorse e dei gruppi di utenti chiusi](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Creazione di un gruppo utenti chiuso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentazione Oak Closed User Group](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
