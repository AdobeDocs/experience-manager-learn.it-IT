---
title: Comprendere il supporto dell'autenticazione in AEM
description: 'Una visione consolidata dei meccanismi di autenticazione (e di autorizzazione occasionalmente) supportati da AEM. '
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
translation-type: tm+mt
source-git-commit: 703321483454435e33c0aa26d66110271fc095d8
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---


# Informazioni sul supporto dell&#39;autenticazione in AEM 6.x

Una visione consolidata dei meccanismi di autenticazione (e di autorizzazione occasionalmente) supportati da AEM.

*La tabella seguente descrive come gli utenti possono effettuare l&#39;autenticazione in AEM.*

<table>
    <tbody>
        <tr>
            <td><strong>Autenticazione</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4  </strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM come provider di identità canonico</strong></td>
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
            <td>Basato su token (con token <a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">incapsulato</a>)</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Sistema non AEM come provider di identità canonico</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a e 2.0</a></td>
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

⁕ *fornito tramite progetti comunitari, ma non direttamente supportato dal Adobe .*
