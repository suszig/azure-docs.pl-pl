---
title: Routing zawartości oparty na adresach URL — omówienie | Microsoft Docs
description: Ta strona zawiera omówienie routingu zawartości opartego na adresach URL, konfiguracji UrlPathMap i reguły PathBasedRouting w usłudze Application Gateway.
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: gwallace

---
# <a name="url-path-based-routing-overview"></a>Routing oparty na ścieżkach URL — omówienie
Routing oparty na ścieżkach URL umożliwia kierowanie ruchu do pul serwerów zaplecza na podstawie ścieżek URL żądania. Jeden ze scenariuszy polega na kierowaniu żądań dla różnych typów zawartości do różnych pól serwerów zaplecza.
W poniższym przykładzie usługa Application Gateway obsługuje ruch dla domeny contoso.com z trzech pul serwerów zaplecza, na przykład: VideoServerPool, ImageServerPool i DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Żądania dla adresu http://contoso.com/video* są kierowane do puli VideoServerPool, a dla adresu http://contoso.com/images* do puli ImageServerPool. Pula DefaultServerPool jest wybierana, jeśli żaden z wzorców ścieżki nie pasuje.

## <a name="urlpathmap-configuration-element"></a>Element konfiguracji UrlPathMap
Element UrlPathMap jest używany do określania wzorców ścieżki na potrzeby mapowań pul serwerów zaplecza. Poniższy przykład kodu jest fragmentem elementu urlPathMap z pliku szablonu.

    "urlPathMaps": [
    {
    "name": "<urlPathMapName>",
    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName>"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpSettingsList/<settingsName>"
        },
        "pathRules": [
            {
                "paths": [
                    <pathPattern>
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName2>"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpsettingsList/<settingsName2>"
                },

            },

        ],

    }
    }


> [!NOTE]
> PathPattern: to ustawienie to lista wzorców ścieżek, które trzeba dopasować. Każdy wzorzec musi zaczynać się od znaku „/”, a znak gwiazdki „*” jest dozwolony jedynie na końcu po znaku „/”. Ciąg przekazywany do narzędzia dopasowywania ścieżki nie zawiera żadnego tekstu po pierwszym znaku „?” lub „#” i te znaki nie są tu dozwolone. 
> 
> 

Aby uzyskać więcej informacji, zobacz [Resource Manager template using URL-based routing](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) (Szablon usługi Resource Manager korzystający z routingu opartego na adresach URL).

## <a name="pathbasedrouting-rule"></a>Reguła PathBasedRouting
Reguła RequestRoutingRule typu PathBasedRouting jest używana do powiązania odbiornika z elementem urlPathMap. Wszystkie żądania otrzymane dla tego odbiornika są kierowane zgodnie z zasadami określonymi w elemencie urlPathMap.
Fragment reguły PathBasedRouting:

    "requestRoutingRules": [
    {

    "name": "<ruleName>",
    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/requestRoutingRules/<ruleName>",
    "properties": {
        "ruleType": "PathBasedRouting",
        "httpListener": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/httpListeners/<listenerName>"
        },
        "urlPathMap": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>"
        },

    }

## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z informacjami na temat routingu zawartości opartego na adresach URL skorzystaj z informacji dotyczących [tworzenia bramy aplikacji przy użyciu routingu opartego na adresach URL](application-gateway-create-url-route-portal.md), aby utworzyć bramę aplikacji za pomocą reguł routingu adresów URL.

<!--HONumber=Oct16_HO3-->


