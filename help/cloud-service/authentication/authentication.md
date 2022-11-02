---
title: Autenticazione in AEM as a Cloud Service
description: Scopri l’autenticazione in AEM di as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
source-git-commit: ad9aa172d37741207dabcbc705efaa851fd17e7c
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 3%

---

# Autenticazione as a Cloud Service AEM

AEM as a Cloud Service supporta più opzioni di autenticazione e varia in base al tipo di servizio.

|  | Autore AEM | AEM Publish |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ↓ | ✘ |
| ・ [SAML 2.0 tramite Adobe IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ↓ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ↓ |
| [Autenticazione token](../../headless-tutorial/authentication/overview.md) | ↓ | ↓ |

## Opzioni di autenticazione

Fai clic sul collegamento corrispondente qui sotto a per informazioni su come impostare e utilizzare l’approccio di autenticazione.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Gestisci l’accesso AEM Author tramite Adobe IMS tramite Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Autentica l'utente del tuo sito web in un IDP utilizzando l'integrazione SAML 2.0 del servizio AEM Publish.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Autenticazione token</a></strong></div>
      <p>
        Consenti alle applicazioni e ai middleware di eseguire l'autenticazione per AEM utilizzando un token del servizio API.
      </p>
    </td>   
  </tr>
</table>
