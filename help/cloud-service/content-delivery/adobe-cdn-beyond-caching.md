---
title: 'CDN Adobe: funzioni avanzate oltre il caching'
description: Scopri le funzioni avanzate di Adobe CDN oltre la memorizzazione in cache, ad esempio la configurazione del traffico sulla CDN, la configurazione di token e credenziali, le pagine di errore CDN e altro ancora.
version: Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: 65e77a329226ca7c7ccb1e583d2a045074feeb3d
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---


# CDN Adobe: funzioni avanzate oltre il caching

Scopri le funzioni avanzate della rete CDN (Content Delivery Network) di Adobe, oltre alla memorizzazione in cache, come la configurazione del traffico sulla rete CDN, la configurazione di token e credenziali, le pagine di errore CDN e altro ancora.

Oltre alla memorizzazione in cache dei contenuti, Adobe CDN offre diverse funzioni avanzate che possono aiutare a ottimizzare le prestazioni del sito web. Queste caratteristiche includono:

- Configurazione del traffico sulla rete CDN
- Configurazione delle credenziali e dell’autenticazione CDN
- Pagine di errore CDN

Queste funzionalità sono **funzionalità self-service**. Configurato nel file `cdn.yaml` del progetto AEM e distribuito utilizzando la pipeline di configurazione Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3433104?quality=12&learn=on)

## Configurazione del traffico sulla rete CDN

Comprendiamo le funzionalità chiave relative a _Configurazione del traffico sulla rete CDN_:

- **Prevenzione attacchi DoS:** la rete CDN Adobe assorbe gli attacchi DoS a livello di rete, impedendo loro di raggiungere il server di origine.
- **Limitazione di frequenza:** Per evitare che il server di origine venga sovraccaricato da un numero eccessivo di richieste, è possibile configurare la limitazione di frequenza nella rete CDN.
- **Firewall applicazione Web (WAF):** WAF protegge il sito Web da vulnerabilità comuni alle applicazioni Web, ad esempio SQL injection, vulnerabilità cross-site scripting e altro ancora. Per utilizzare questa funzione è necessaria la licenza Protezione avanzata o la licenza Protezione WAF-DDoS.
- **Trasformazione richiesta:** Modificare le richieste in ingresso, ad esempio impostando o disimpostando le intestazioni, modificando i parametri di query, i cookie e altro ancora.
- **Trasformazione risposta:** Modificare le risposte in uscita, ad esempio impostando o disattivando le intestazioni.
- **Selezione origine:** Instrada il traffico a server di origine diversi (Adobi e non Adobi) in base all&#39;URL della richiesta.
- **Reindirizzamento URL:** richieste di reindirizzamento (HTTP 301/302) a un URL assoluto o relativo diverso.

## Configurazione delle credenziali e dell’autenticazione CDN

Comprendiamo le funzionalità chiave relative a _Configurazione delle credenziali CDN e dell&#39;autenticazione_:

- **Rimuovi token API**: consente di creare una chiave di eliminazione personalizzata per rimuovere un singolo o un gruppo o tutte le risorse dalla cache.
- **Autenticazione di base**: meccanismo di autenticazione leggero che consente di limitare l&#39;accesso al sito Web o a parte di esso. Richiesto principalmente come parte di vari processi di revisione prima di andare &quot;live&quot;.
- **Convalida intestazione HTTP**: utilizzato quando una rete CDN gestita dal cliente instrada il traffico verso la rete CDN Adobe. Il CDN di Adobe convalida la richiesta in ingresso in base al valore dell&#39;intestazione `X-AEM-Edge-Key`. Consente di creare un valore personalizzato per l&#39;intestazione `X-AEM-Edge-Key`.

## Pagine di errore CDN

Comprendiamo le funzionalità chiave relative alle _pagine di errore CDN_:

- **Pagine di errore con marchio**: visualizza una pagina di errore con marchio agli utenti nello _scenario improbabile_ quando la rete CDN di Adobe non è in grado di raggiungere il server di origine.

## Come implementare

L’implementazione di queste funzioni avanzate prevede due passaggi:

1. **Aggiorna file di configurazione CDN**: aggiorna il file `cdn.yaml` nel progetto AEM con le configurazioni richieste. Le configurazioni vengono aggiunte come regole e seguono una sintassi di regola. La regola prevede tre componenti principali: `name`, `when` e `action`.

2. **Distribuisci file di configurazione CDN**: distribuisci il file `cdn.yaml` aggiornato utilizzando la pipeline di configurazione di Cloud Manager. Per ulteriori informazioni, vedere [Distribuire le regole tramite Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

### Esempio

Nell&#39;esempio seguente, il sito WKND di esempio è configurato per reindirizzare l&#39;URL `/top3` a `/us/en/top3.html`.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  experimental_redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## Tutorials correlati

[Protezione dei siti Web con regole filtro traffico](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[Configura e distribuisci la regola CDN di convalida dell&#39;intestazione HTTP](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[Eliminare la cache CDN](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[Configurazione del traffico sulla rete CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[Configurazione credenziali e autenticazione CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

[Configurazione delle pagine di errore CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-error-pages)
