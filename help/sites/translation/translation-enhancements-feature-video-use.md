---
title: Miglioramenti alla traduzione in AEM
description: AEM robusto framework di traduzione consente di tradurre i contenuti AEM senza soluzione di continuità da parte dei fornitori di traduzione supportati. Scopri gli ultimi miglioramenti.
version: 6.4, 6.5
topic: Localization
feature: Multi Site Manager, Language Copy
role: User
level: Beginner
exl-id: 21633308-ffe4-4023-affe-59269504da69
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 5%

---

# Miglioramenti alla traduzione con Multi-Site Manager {#translation-enhancements}

AEM robusto framework di traduzione consente di tradurre i contenuti AEM senza soluzione di continuità da parte dei fornitori di traduzione supportati.

## Miglioramenti alla traduzione in AEM 6.5

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=12&learn=on)

I miglioramenti alla traduzione AEM 6.5 includono:

**Approvazione automatica dei processi di traduzione**: Il flag di approvazione nel processo di traduzione è una proprietà binaria. Non supporta né integra con flussi di lavoro di revisione e approvazione preconfigurati. Per ridurre al minimo il numero di passaggi di un processo di traduzione, per impostazione predefinita viene impostato su &quot;approva automaticamente&quot; in [!UICONTROL Proprietà avanzate] di un progetto di traduzione. Se l&#39;organizzazione richiede l&#39;approvazione per un processo di traduzione, è possibile deselezionare l&#39;opzione &quot;approva automaticamente&quot; in [!UICONTROL Proprietà avanzate] di un progetto di traduzione.

**Elimina automaticamente i lanci di traduzione**: Invece di eliminare manualmente i lanci di traduzione in Lanci Admin dopo il fatto, ora è possibile eliminare automaticamente i lanci di traduzione dopo che sono stati promossi.

**Esportare oggetti di traduzione in formato JSON**: AEM 6.4 e versioni precedenti supportano i formati XML e XLIFF degli oggetti di traduzione. Ora puoi configurare il formato di esportazione in formato JSON utilizzando la console dei sistemi [!UICONTROL Gestione configurazione]. Cerca [!UICONTROL Configurazione della piattaforma di traduzione], quindi puoi selezionare il formato di esportazione come JSON.

**Aggiorna il contenuto AEM tradotto nella memoria di traduzione (TMS)**: l’autore locale che non ha accesso a AEM può apportare aggiornamenti ai contenuti tradotti, che sono già stati acquisiti in AEM, direttamente nel TM (memoria di traduzione, in TMS), e aggiornare le traduzioni in AEM inviando nuovamente il lavoro di traduzione da TMS a AEM

## Miglioramenti alla traduzione in AEM 6.4

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=12&learn=on)

Gli autori possono ora creare in modo rapido e semplice progetti di traduzione multilingue direttamente dall’amministratore di Sites o dall’amministratore di Projects, impostare questi progetti per promuovere automaticamente i lanci e anche impostare le pianificazioni per l’automazione.

## Risorse aggiuntive {#additional-resources}

* [Traduzione di contenuti per siti multilingue](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Best practice per la traduzione](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
