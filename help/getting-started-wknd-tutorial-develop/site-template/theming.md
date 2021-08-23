---
title: Flusso di lavoro dei temi
seo-title: Guida introduttiva ad AEM Sites - Flusso di lavoro dei temi
description: Scopri come aggiornare le origini dei temi di un sito Adobe Experience Manager per applicare stili specifici per il marchio. Scopri come utilizzare un server proxy per visualizzare un’anteprima live degli aggiornamenti CSS e JavaScript. Questa esercitazione illustra anche come distribuire gli aggiornamenti a un tema in un sito AEM utilizzando le azioni GitHub.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Componenti core
topic: Gestione dei contenuti, sviluppo
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---


# Flusso di lavoro dei temi {#theming}

>[!CAUTION]
>
> Le funzionalità di creazione rapida dei siti mostrate qui saranno rilasciate nella seconda metà del 2021. La relativa documentazione è disponibile a scopo di anteprima.

In questo capitolo verranno aggiornate le origini dei temi di un sito Adobe Experience Manager per applicare stili specifici per il marchio. Scopriremo come utilizzare un server proxy per visualizzare un’anteprima degli aggiornamenti CSS e Javascript mentre effettuiamo il codice per il sito live. Questa esercitazione illustra anche come distribuire gli aggiornamenti a un tema in un sito AEM utilizzando le azioni GitHub.

Alla fine il nostro sito verrà aggiornato per includere gli stili che corrispondono al marchio WKND.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che i passaggi descritti nel capitolo [Modelli di pagina](./page-templates.md) siano stati completati.

## Obiettivi

1. Scopri come scaricare e modificare le origini dei temi di un sito.
1. Scopri come eseguire il codice rispetto al sito live per un’anteprima in tempo reale.
1. Comprendi il flusso di lavoro end-to-end della distribuzione degli aggiornamenti CSS e JavaScript compilati come parte di un tema utilizzando le azioni GitHub.

## Aggiornare un tema {#theme-update}

Quindi, apporta modifiche alle origini tema in modo che il sito corrisponda all’aspetto del marchio WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Passi di alto livello per il video:

1. Crea un utente locale in AEM da utilizzare con un server di sviluppo proxy.
1. Scarica le origini di tema da AEM e apri utilizzando un IDE locale, come VSCode.
1. Modifica le origini dei temi e utilizza un server di sviluppo proxy per visualizzare in anteprima le modifiche CSS e JavaScript in tempo reale.
1. Aggiorna le origini dei temi in modo che l&#39;articolo della rivista corrisponda agli stili e ai modelli con brand WKND.

### File della soluzione

Scarica gli stili finiti per il [tema WKND](assets/theming/WKND-THEME-src.zip)

## Distribuire un tema {#deploy-theme}

Distribuire aggiornamenti a un tema in un ambiente AEM utilizzando le azioni GitHub.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Passi di alto livello per il video:

1. Aggiungi il progetto origini tema a [GitHub come nuovo archivio](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line).
1. Crea [un token di accesso personale in GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) e salva in una posizione sicura.
1. Configura le impostazioni GitHub nell’ambiente AEM per puntare all’archivio GitHub e includere il token di accesso personale.
1. Crea i seguenti [segreti crittografati](https://docs.github.com/en/actions/reference/encrypted-secrets) nel tuo archivio GitHub:
   * **AEM_SITE** : radice del sito AEM (ad es.  `wknd`)
   * **AEM_URL** : url dell’ambiente di authoring di AEM
   * **GIT_TOKEN**  - Token di accesso personale dal passaggio 2.
1. Esegui l’azione GitHub: **Genera e distribuisci gli artefatti Github**. Ciò avrà l&#39;effetto a valle dell&#39;esecuzione dell&#39;azione: **Aggiorna la configurazione del tema in AEM con artifact id**, che distribuirà le origini del tema nell&#39;ambiente AEM come specificato da `AEM_URL` e `AEM_SITE`.

### Operazioni di pronti contro termine di esempio

Ci sono un paio di esempi di repository GitHub che possono essere utilizzati come riferimento:

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  - Utilizzato come esempio per progetti &quot;reali&quot;.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  - Utilizzato come esempio per i seguenti tutorial.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato un tema aggiornato e distribuito da AEM!

### Passaggi successivi {#next-steps}

Approfondisci AEM lo sviluppo e scopri di più sulla tecnologia sottostante creando un sito utilizzando il [AEM Project Archetype](../project-archetype/overview.md).
