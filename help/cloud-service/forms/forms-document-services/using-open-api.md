---
title: Configurare l’API di comunicazione di AEM Forms
description: Configurare le API di comunicazione AEM Forms basate su OpenAPI per l’autenticazione server-to-server
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

---

# Configurare le API di comunicazione AEM Forms basate su OpenAPI su AEM Forms as a Cloud Service

## Prerequisiti

* Istanza più recente di AEM Forms as a Cloud Service.
* Tutti i profili di prodotto [ necessari vengono aggiunti all&#39;ambiente.](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* Abilita l’accesso API di AEM al profilo di prodotto come mostrato di seguito
  ![profilo_prodotto1](assets/product-profiles1.png)
  ![profilo_prodotto](assets/product-profiles.png)

## Crea progetto Adobe Developer Console

Accedi a [Adobe Developer Console](https://developer.adobe.com/console/) utilizzando il tuo Adobe ID.
Crea un nuovo progetto facendo clic sull’icona appropriata
![nuovo-progetto](assets/new-project.png)

Assegna un nome significativo al progetto e fai clic sull’icona Aggiungi API
![nuovo-progetto](assets/new-project2.png)

Seleziona Experience Cloud
![nuovo-progetto3](assets/new-project3.png)
Seleziona AEM Forms Communications API e fai clic su Avanti
![nuovo-progetto4](assets/new-project4.png)

Verificare di aver selezionato l&#39;autenticazione server-to-server e fare clic su Avanti
![nuovo-progetto5](assets/new-project5.png)
Seleziona i profili e fai clic sul pulsante Salva API configurata per salvare le impostazioni
![nuovo-progetto6](assets/new-project6.png)
Fai clic sul server OAuth
![nuovo-progetto7](assets/new-project7.png)
Copia l’ID client, il segreto client e gli ambiti
![nuovo-progetto8](assets/new-project8.png)

## Configura istanza AEM per abilitare la comunicazione del progetto ADC

Se disponi già di un progetto AEM Forms, [segui queste istruzioni](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) per abilitare l&#39;ID client delle credenziali server-to-server OAuth del progetto Adobe Developer Console per comunicare con l&#39;istanza AEM

Se non disponi di un progetto AEM Forms, crea un progetto AEM Forms [ seguendo questa documentazione.](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) e quindi abilitare l&#39;ID client delle credenziali server-to-server OAuth del progetto Adobe Developer Console per comunicare con l&#39;istanza AEM [utilizzando questa documentazione.](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## Passaggi successivi

[Genera token di accesso](./generate-access-token.md)
