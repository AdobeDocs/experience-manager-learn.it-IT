---
title: Impostare le regole di traduzione in AEM
description: L’interfaccia utente Configurazione traduzione consente a un utente di gestire le regole per la traduzione di contenuti in AEM Sites. Questo video illustra la creazione di una nuova regola di traduzione per un componente personalizzato.
feature: Copia lingua
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localizzazione
role: Professionista
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 3%

---


# Imposta regole di traduzione {#set-up-translation-rules-in-aem}

L’interfaccia utente Configurazione traduzione consente a un utente di gestire le regole per la traduzione di contenuti in AEM Sites. Questo video illustra la creazione di una nuova regola di traduzione per un componente personalizzato.

>[!NOTE]
>
> Il video seguente è stato registrato su AEM 6.3. AEM 6.4+ introduce una nuova struttura di archivio per la memorizzazione del file XML delle regole di traduzione. Quando utilizzi l’interfaccia utente di configurazione della traduzione in AEM 6.4+, le regole vengono salvate nel percorso `/conf/global/settings/translation/rules/translation_rules.xml`. Per ulteriori informazioni, consulta [Identificazione del contenuto da tradurre](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) .

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

Le regole di traduzione identificano il contenuto in AEM da estrarre per la traduzione. Le regole di traduzione predefinite coprono casi d’uso comuni, come i componenti Testo e il testo Alt per i componenti Immagine. A seconda di un progetto di traduzione potrebbero essere necessarie ulteriori regole. In generale, le regole di traduzione consentono agli utenti di specificare:

1. Proprietà da tradurre in base al percorso e/o al tipo di risorsa
2. Filtri per le proprietà che NON devono essere tradotti
3. Contenuto di riferimento da tradurre (ad esempio immagini o frammenti di contenuto)

Editor delle regole di traduzione che aggiornerà il file xml di traduzione. L’interfaccia utente di configurazione della traduzione facilita la gestione di varie regole di traduzione e protegge dagli errori di battitura quando si modifica direttamente XML.

Accedi all’interfaccia utente di configurazione della traduzione:

* **[!UICONTROL Menu]  Start di AEM >  [!UICONTROL Strumenti]  >  [!UICONTROL Generale]  > Configurazione della  [[!UICONTROL traduzione]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Prima di AEM 6.3 {#prior-to-aem}

Nelle precedenti regole di traduzione della versione AEM venivano aggiornate manualmente modificando un file XML che si trova nel flusso di lavoro di traduzione: `/etc/workflow/models/translation/translation_rules.xml`.

## Risorse aggiuntive {#additional-resources}

* [Identificazione del contenuto da tradurre](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traduzione di contenuti per siti multilingue](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Best practice per la traduzione](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
