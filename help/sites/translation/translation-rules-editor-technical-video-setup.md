---
title: Imposta regole di traduzione in AEM
description: L'interfaccia utente Configurazione traduzione consente a un utente di gestire le regole per la traduzione del contenuto in  AEM Sites. Questo video descrive in dettaglio la creazione di una nuova regola di traduzione per un componente personalizzato.
feature: language-copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---


# Imposta regole di traduzione {#set-up-translation-rules-in-aem}

L&#39;interfaccia utente Configurazione traduzione consente a un utente di gestire le regole per la traduzione del contenuto in  AEM Sites. Questo video descrive in dettaglio la creazione di una nuova regola di traduzione per un componente personalizzato.

>[!NOTE]
>
> Il video seguente è stato registrato il AEM 6.3. AEM 6.4+ introduce una nuova struttura di repository per la memorizzazione del file XML delle regole di conversione. Quando si utilizza l&#39;interfaccia utente Configurazione traduzione in AEM 6.4+, le regole vengono salvate nel percorso `/conf/global/settings/translation/rules/translation_rules.xml`. Per ulteriori informazioni, vedere [Identificazione dei contenuti da tradurre](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html).

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

Le regole di traduzione identificano il contenuto in AEM da estrarre per la traduzione. Le regole di conversione predefinite coprono i casi d’uso più comuni, ad esempio i componenti Testo e il testo Alt per i componenti Immagine. A seconda di un progetto di traduzione requisiti ulteriori potrebbe essere necessario. In generale, le regole di traduzione consentono agli utenti di specificare:

1. Proprietà da convertire in base al percorso e/o al tipo di risorsa
2. Filtri per proprietà che NON devono essere tradotte
3. Contenuto di riferimento da convertire (ad esempio, immagini o frammenti di contenuto)

Editor delle regole di conversione che aggiornerà il file xml di traduzione. L’interfaccia utente Configurazione traduzione facilita la gestione di varie regole di traduzione e protegge dagli errori di battitura quando si modifica direttamente XML.

Accedete all’interfaccia utente Configurazione traduzione:

* **[!UICONTROL AEM menu]  Start >  [!UICONTROL Strumenti]  >  [!UICONTROL Generale]  > Configurazione  [[!UICONTROL traduzione]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Prima di AEM 6.3 {#prior-to-aem}

Nelle versioni precedenti AEM regole di traduzione venivano aggiornate manualmente modificando un file XML che si trova nel flusso di lavoro di traduzione: `/etc/workflow/models/translation/translation_rules.xml`.

## Risorse aggiuntive {#additional-resources}

* [Identificazione del contenuto da tradurre](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traduzione di contenuti per siti multilingue](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Tecniche consigliate per la traduzione](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
