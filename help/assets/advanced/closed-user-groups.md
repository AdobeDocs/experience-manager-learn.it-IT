---
title: Gruppi di utenti chiusi in AEM Assets
description: I gruppi chiusi di utenti (CUG) sono una funzione utilizzata per limitare l’accesso al contenuto a un gruppo selezionato di utenti su un sito pubblicato. Questo video mostra come i gruppi chiusi di utenti possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 321
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Gruppi utenti chiusi{#using-closed-user-groups-with-aem-assets}

I gruppi chiusi di utenti (CUG) sono una funzione utilizzata per limitare l’accesso al contenuto a un gruppo selezionato di utenti su un sito pubblicato. Questo video mostra come i gruppi chiusi di utenti possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse. Il supporto per gruppi chiusi di utenti con AEM Assets è stato introdotto per la prima volta in AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/3410276?quality=12&learn=on&captions=ita)

## Gruppo utenti chiuso (CUG) con AEM Assets

* Progettato per limitare l’accesso alle risorse in un’istanza AEM Publish.
* Consente l’accesso in lettura a un set di utenti/gruppi.
* Il gruppo utenti chiusi (CUG) può essere configurato solo a livello di cartella. Impossibile impostare CUG su singole risorse.
* I criteri CUG vengono ereditati automaticamente da tutte le sottocartelle e le risorse applicate.
* I criteri CUG possono essere sostituiti da sottocartelle impostando un nuovo criterio CUG. Questo dovrebbe essere usato con moderazione e non è considerato una best practice.

## Gruppi utenti chiusi ed elenchi di controllo di accesso {#closed-user-groups-vs-access-control-lists}

Sia i gruppi chiusi di utenti (CUG) che gli elenchi di controllo di accesso (ACL) vengono utilizzati per controllare l’accesso al contenuto in AEM e in base agli utenti e ai gruppi di sicurezza di AEM. Tuttavia, l’applicazione e l’implementazione di queste funzioni sono molto diverse. Nella tabella seguente vengono riepilogate le distinzioni tra le due feature.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configura e applica le autorizzazioni per il contenuto nell&#39;istanza di AEM **corrente**. | Configura i criteri per i gruppi utenti chiusi (CUG) per il contenuto nell&#39;istanza **author** di AEM. Applica criteri CUG per il contenuto nelle istanze **publish** di AEM. |
| Livelli di autorizzazione | Definisce le autorizzazioni concesse/negate per utenti/gruppi per tutti i livelli: Lettura, Modifica, Crea, Elimina, Lettura ACL, Modifica ACL, Replica. | Consente l’accesso in lettura a un set di utenti/gruppi. Nega l&#39;accesso in lettura a *tutti gli altri* utenti/gruppi. |
| Pubblicazione | Gli ACL sono *non* pubblicati con il contenuto. | I criteri CUG *sono* pubblicati con contenuto. |

## Collegamenti di supporto {#supporting-links}

* [Gestione di Assets e gruppi di utenti chiusi](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=it#closed-user-group)
* [Creazione di un gruppo utenti chiuso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html?lang=it)
* [Documentazione di Oak Closed User Group](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
