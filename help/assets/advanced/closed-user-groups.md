---
title: Gruppi di utenti chiusi in  AEM Assets
description: 'Gruppi di utenti chiusi (CUG) è una funzione utilizzata per limitare l’accesso ai contenuti a un determinato gruppo di utenti in un sito pubblicato. Questo video mostra come i gruppi di utenti chiusi possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse. Il supporto per i gruppi di utenti chiusi con  AEM Assets è stato introdotto per la prima volta in AEM 6.4. '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---


# Gruppi di utenti chiusi{#using-closed-user-groups-with-aem-assets}

Gruppi di utenti chiusi (CUG) è una funzione utilizzata per limitare l’accesso ai contenuti a un determinato gruppo di utenti in un sito pubblicato. Questo video mostra come i gruppi di utenti chiusi possono essere utilizzati con Adobe Experience Manager Assets per limitare l’accesso a una specifica cartella di risorse. Il supporto per i gruppi di utenti chiusi con  AEM Assets è stato introdotto per la prima volta in AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## Gruppo utenti chiuso (CUG) con  AEM Assets

* Progettato per limitare l&#39;accesso alle risorse su un&#39;istanza AEM Publish.
* Consente l&#39;accesso in lettura a un set di utenti/gruppi.
* È possibile configurare CUG solo a livello di cartella. Impossibile impostare CUG per singole risorse.
* I criteri CUG vengono ereditati automaticamente dalle sottocartelle e dalle risorse applicate.
* I criteri CUG possono essere sostituiti dalle sottocartelle impostando un nuovo criterio CUG. Questo dovrebbe essere utilizzato con cautela e non è considerata una best practice.

## Rappresentazione CUG in JCR {#cug-representation-in-the-jcr}

![Rappresentazione CUG nel JCR](assets/closed-user-groups/folder-properties-closed-user-groups.png)

We.Retail Members Group aggiunto come gruppo di utenti chiuso alla cartella: /content/dam/we-retail/en/beta-products

Un mixin di **rep:CugMixin** viene applicato alla cartella **/content/dam/we-retail/en/beta-products**. Un nodo di **rep:cugPolicy** viene aggiunto sotto la cartella e i membri di commercio al dettaglio vengono specificati come entità. Un altro mix di **granite:AuthenticationRequired** viene applicato alla cartella beta-products e la proprietà** granite:loginPath** specifica la pagina di login da utilizzare se un utente non è autenticato e tenta di richiedere una risorsa sotto la cartella **beta-products**.

Descrizione JCR seguente:

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## Gruppi di utenti chiusi e elenchi di controllo degli accessi {#closed-user-groups-vs-access-control-lists}

Sia i gruppi di utenti chiusi (CUG) che gli elenchi di controllo degli accessi (ACL) vengono utilizzati per controllare l&#39;accesso al contenuto in AEM e basati su AEM utenti e gruppi di sicurezza. Tuttavia, l&#39;applicazione e l&#39;implementazione di queste funzioni è molto diversa. Nella tabella seguente sono riepilogate le differenze tra le due funzioni.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configura e applica le autorizzazioni per il contenuto nell&#39;istanza di AEM **corrente**. | Configurare i criteri CUG per il contenuto nell&#39;istanza AEM **author**. Applicate criteri CUG per il contenuto nelle istanze AEM **publish**. |
| Livelli di autorizzazione | Definisce le autorizzazioni concesse/negate per utenti/gruppi per tutti i livelli: Lettura, Modifica, Crea, Elimina, Lettura ACL, Modifica ACL, Replica. | Consente l&#39;accesso in lettura a un set di utenti/gruppi. Rifiuta l’accesso in lettura a tutti gli altri utenti/gruppi. |
| Replica | Gli ACL non vengono replicati con il contenuto. | I criteri CUG vengono replicati con il contenuto. |

## Collegamenti di supporto {#supporting-links}

* [Gestione delle risorse e dei gruppi di utenti chiusi](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [Creazione di un gruppo utenti chiuso](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Documentazione gruppo utenti chiuso Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
