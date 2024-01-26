---
title: Gruppi di utenti chiusi in AEM Assets
description: I gruppi chiusi di utenti (CUG) sono una funzione utilizzata per limitare l’accesso al contenuto a un gruppo selezionato di utenti su un sito pubblicato. Questo video mostra come i gruppi chiusi di utenti possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 340
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Gruppi utenti chiusi{#using-closed-user-groups-with-aem-assets}

I gruppi chiusi di utenti (CUG) sono una funzione utilizzata per limitare l’accesso al contenuto a un gruppo selezionato di utenti su un sito pubblicato. Questo video mostra come i gruppi chiusi di utenti possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse. Il supporto per gruppi chiusi di utenti con AEM Assets è stato introdotto per la prima volta nell’AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Gruppo utenti chiuso (CUG) con AEM Assets

* Progettato per limitare l’accesso alle risorse in un’istanza di pubblicazione AEM.
* Consente l’accesso in lettura a un set di utenti/gruppi.
* Il gruppo utenti chiusi (CUG) può essere configurato solo a livello di cartella. Impossibile impostare CUG su singole risorse.
* I criteri CUG vengono ereditati automaticamente da tutte le sottocartelle e le risorse applicate.
* I criteri CUG possono essere sostituiti da sottocartelle impostando un nuovo criterio CUG. Questo dovrebbe essere usato con moderazione e non è considerato una best practice.

## Gruppi utenti chiusi ed elenchi di controllo di accesso {#closed-user-groups-vs-access-control-lists}

Sia i gruppi chiusi di utenti (CUG) che gli elenchi di controllo di accesso (ACL) vengono utilizzati per controllare l’accesso al contenuto in AEM e in base agli utenti e ai gruppi di sicurezza AEM. Tuttavia, l’applicazione e l’implementazione di queste funzioni sono molto diverse. Nella tabella seguente vengono riepilogate le distinzioni tra le due feature.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configurare e applicare le autorizzazioni per il contenuto in **corrente** istanza AEM. | Configurare i criteri CUG per il contenuto su AEM **autore** dell&#39;istanza. Applicare criteri CUG per i contenuti su AEM **pubblicare** istanze. |
| Livelli di autorizzazione | Definisce le autorizzazioni concesse/negate per utenti/gruppi per tutti i livelli: Lettura, Modifica, Crea, Elimina, Lettura ACL, Modifica ACL, Replica. | Consente l’accesso in lettura a un set di utenti/gruppi. Nega l’accesso in lettura a *tutti gli altri* utenti/gruppi. |
| Pubblicazione | Gli ACL sono *non* pubblicato con il contenuto. | Criteri CUG *sono* pubblicato con il contenuto. |

## Collegamenti di supporto {#supporting-links}

* [Gestione delle risorse e dei gruppi chiusi di utenti](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Creazione di un gruppo utenti chiuso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentazione di Oak Closed User Group](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
