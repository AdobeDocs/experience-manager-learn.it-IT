---
title: Utilizzo della traduzione con AEM frammenti di contenuto
description: AEM 6.3 introduce la possibilità di tradurre i frammenti di contenuto. È inoltre possibile estrarre e convertire le risorse multimediali e le raccolte di risorse associate a un frammento di contenuto.
sub-product: siti, risorse, servizi di contenuto
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# Utilizzo della traduzione con AEM frammenti di contenuto{#using-translation-with-aem-content-fragments}

AEM 6.3 introduce la possibilità di tradurre i frammenti di contenuto. È inoltre possibile estrarre e convertire le risorse multimediali e le raccolte di risorse associate a un frammento di contenuto.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## Casi di utilizzo di conversione di frammento di contenuto {#content-fragment-translation-use-cases}

I frammenti di contenuto sono un tipo di contenuto riconosciuto che AEM verrà estratto per essere inviato a un servizio di traduzione esterno. Sono supportati diversi casi di utilizzo:

1. Un frammento di contenuto può essere selezionato direttamente nella console Risorse per la copia e la traduzione in lingua
2. I frammenti di contenuto a cui si fa riferimento in una pagina Siti verranno copiati nella cartella della lingua appropriata ed estratti per la traduzione quando la pagina Siti viene selezionata per la copia della lingua
3. Le risorse multimediali in linea incorporate in un frammento di contenuto possono essere estratte e tradotte.
4. Le raccolte di risorse associate a un frammento di contenuto possono essere estratte e tradotte

## Opzioni di configurazione della traduzione {#translation-config-options}

La configurazione di traduzione out-of-box supporta diverse opzioni per la traduzione di frammenti di contenuto. Per impostazione predefinita, le risorse multimediali in linea e le raccolte di risorse associate NON sono convertite. Per aggiornare la configurazione di conversione, andate a [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html).

Sono disponibili quattro opzioni per tradurre le risorse Frammento di contenuto:

1. **Non tradurre (predefinito)**
2. **Solo risorse multimediali in linea**
3. **Solo raccolte risorse associate**
4. **Risorse multimediali in linea e raccolte associate**

![Configurazione traduzione](assets/classic-ui-dialog.png)
