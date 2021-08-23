---
title: Comprendere il supporto per l’autenticazione in AEM
description: 'Una visione consolidata dei meccanismi di autenticazione (e di autorizzazione occasionalmente) supportati da AEM. '
version: 6.3, 6.4, 6.5
feature: Utenti e gruppi
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
topic: Architettura
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 3%

---


# Comprendere il supporto per l&#39;autenticazione in AEM 6.x

Una visione consolidata dei meccanismi di autenticazione (e di autorizzazione occasionalmente) supportati da AEM.

*La tabella seguente descrive come gli utenti possono eseguire l’autenticazione in AEM.*

<table>
    <tbody>
        <tr>
            <td><strong>Autenticazione</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM come fornitore di identità canonica</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>Autenticazione di base</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td>Basato su Forms</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td>Basato su token (con <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">token incapsulato</a>)</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Sistema non AEM come fornitore di identità canonica</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>↓</td>
                <td>↓</td>
                <td>↓</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
                <td>↓</td>
                <td>↓</td>
                <td>↓</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>↓</td>
                <td>↓</td>
                <td>↓</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a e 2.0</a></td>
                <td>↓</td>
                <td>↓</td>
                <td>↓</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

⁕ *Forniti tramite progetti della community, ma non direttamente supportati da Adobe.*
