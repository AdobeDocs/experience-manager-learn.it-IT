---
title: Supporto dell’autenticazione in AEM 6.x
description: Una vista consolidata dei meccanismi di autenticazione supportati da AEM 6.x.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Article
jira: KT-406
topic: Architecture
role: Developer
level: Experienced
exl-id: 96c542ae-6ab6-4d8a-94df-a58b03469320
last-substantial-update: 2022-09-10T00:00:00Z
thumbnail: KT-406.jpg
duration: 22
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 9%

---

# Supporto dell’autenticazione in AEM 6.x

Una visualizzazione consolidata dei meccanismi di autenticazione (e occasionalmente di autorizzazione) supportati da AEM.

*Nella tabella seguente viene descritto come gli utenti possono eseguire l&#39;autenticazione in AEM.*

<table>
    <tbody>
        <tr>
            <td><strong>Autenticazione</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM come provider di identità canonica</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>Autenticazione di base</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Basato su Forms</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Basato su token (con <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">token incapsulato</a>)</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Sistema non AEM come provider di identità canonica</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html?lang=it" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html?lang=it" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/events/assets/oauth-server-functionality-in-aem-7-23-14.pdf" target="_blank">OAuth 1.0a e 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

⁕ *Fornito tramite progetti community, ma non direttamente supportato da Adobe.*
