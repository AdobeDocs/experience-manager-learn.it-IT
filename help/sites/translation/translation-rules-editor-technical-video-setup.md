---
title: Impostare le regole di traduzione in AEM
description: L’interfaccia utente per la configurazione della traduzione consente a un utente di gestire le regole per la traduzione dei contenuti in AEM Sites. Questo video illustra come creare una nuova regola di traduzione per un componente personalizzato.
feature: Language Copy
version: Experience Manager 6.4, Experience Manager 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 542
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Imposta regole di traduzione {#set-up-translation-rules-in-aem}

L’interfaccia utente per la configurazione della traduzione consente a un utente di gestire le regole per la traduzione dei contenuti in AEM Sites. Questo video illustra come creare una nuova regola di traduzione per un componente personalizzato.

>[!NOTE]
>
> Il video seguente è stato registrato su AEM 6.3. AEM 6.4+ introduce una nuova struttura dell’archivio per l’archiviazione del file XML delle regole di traduzione. Quando si utilizza l&#39;interfaccia utente di configurazione della traduzione in AEM 6.4+, le regole vengono salvate nel percorso `/conf/global/settings/translation/rules/translation_rules.xml`. Per ulteriori dettagli, vedere [Identificazione del contenuto da tradurre](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/tc-rules.html).

>[!VIDEO](https://video.tv.adobe.com/v/41149?quality=12&learn=on&captions=ita)

Le regole di traduzione identificano il contenuto in AEM da estrarre per la traduzione. Le regole di traduzione pronte all’uso coprono casi d’uso comuni come i componenti Testo e il testo alternativo per i componenti Immagine. A seconda dei requisiti di traduzione di un progetto, potrebbero essere necessarie regole aggiuntive. In generale, le regole di traduzione consentono agli utenti di specificare:

1. Proprietà che devono essere tradotte in base al percorso e/o al tipo di risorsa
2. Filtri per le proprietà da NON tradurre
3. Contenuto di riferimento da tradurre (ad esempio immagini o frammenti di contenuto)

L’editor delle regole di traduzione che aggiornerà il file xml di traduzione. L’interfaccia utente per la configurazione della traduzione semplifica la gestione di varie regole di traduzione e impedisce l’utilizzo di errori di battitura durante la modifica diretta di XML.

Accedi all’interfaccia utente per la configurazione della traduzione:

* **[!UICONTROL Menu Start di AEM] > [!UICONTROL Strumenti] > [!UICONTROL Generale] > [[!UICONTROL Configurazione traduzione]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Prima di AEM 6.3 {#prior-to-aem}

Nelle versioni precedenti di AEM le regole di traduzione sono state aggiornate manualmente modificando un file XML che si trova nel flusso di lavoro di traduzione: `/etc/workflow/models/translation/translation_rules.xml`.

## Risorse aggiuntive {#additional-resources}

* [Identificazione del contenuto da tradurre](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traduzione di contenuti per siti multilingue](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Best practice per la traduzione](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/tc-bp.html)
