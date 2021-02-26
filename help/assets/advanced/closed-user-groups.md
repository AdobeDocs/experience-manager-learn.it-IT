---
title: Gruppi di utenti chiusi in  AEM Assets
description: Gruppi di utenti chiusi (CUG) è una funzione utilizzata per limitare l’accesso ai contenuti a un determinato gruppo di utenti in un sito pubblicato. Questo video mostra come i gruppi di utenti chiusi possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse.
version: 6.3, 6.4, 6.5, cloud-service
topic: Amministrazione, sicurezza
feature: Utenti e gruppi
role: Amministratore
level: Intermedio
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 1%

---


# Gruppi di utenti chiusi{#using-closed-user-groups-with-aem-assets}

Gruppi di utenti chiusi (CUG) è una funzione utilizzata per limitare l’accesso ai contenuti a un determinato gruppo di utenti in un sito pubblicato. Questo video mostra come i gruppi di utenti chiusi possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse. Il supporto per i gruppi di utenti chiusi con  AEM Assets è stato introdotto per la prima volta in AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Gruppo utenti chiuso (CUG) con  AEM Assets

* Progettato per limitare l&#39;accesso alle risorse su un&#39;istanza AEM Publish.
* Consente l&#39;accesso in lettura a un set di utenti/gruppi.
* È possibile configurare CUG solo a livello di cartella. Impossibile impostare CUG per singole risorse.
* I criteri CUG vengono ereditati automaticamente dalle sottocartelle e dalle risorse applicate.
* I criteri CUG possono essere sostituiti dalle sottocartelle impostando un nuovo criterio CUG. Questo dovrebbe essere utilizzato con cautela e non è considerata una best practice.

## Gruppi di utenti chiusi e elenchi di controllo degli accessi {#closed-user-groups-vs-access-control-lists}

Sia i gruppi di utenti chiusi (CUG) che gli elenchi di controllo degli accessi (ACL) vengono utilizzati per controllare l&#39;accesso al contenuto in AEM e basati su AEM utenti e gruppi di sicurezza. Tuttavia, l&#39;applicazione e l&#39;implementazione di queste funzioni è molto diversa. Nella tabella seguente sono riepilogate le differenze tra le due funzioni.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configura e applica le autorizzazioni per il contenuto nell&#39;istanza di AEM **corrente**. | Configurare i criteri CUG per il contenuto nell&#39;istanza AEM **author**. Applicate criteri CUG per il contenuto nelle istanze AEM **publish**. |
| Livelli di autorizzazione | Definisce le autorizzazioni concesse/negate per utenti/gruppi per tutti i livelli: Lettura, Modifica, Crea, Elimina, Lettura ACL, Modifica ACL, Replica. | Consente l&#39;accesso in lettura a un set di utenti/gruppi. Rifiuta l&#39;accesso in lettura a *tutti gli altri utenti/gruppi*. |
| Pubblicazione | Gli ACL sono *non* pubblicati con il contenuto. | I criteri CUG *sono* pubblicati con il contenuto. |

## Collegamenti di supporto {#supporting-links}

* [Gestione delle risorse e dei gruppi di utenti chiusi](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Creazione di un gruppo utenti chiuso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentazione gruppo utenti chiuso Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
