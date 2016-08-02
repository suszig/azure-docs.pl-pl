<properties
   pageTitle="Ustawianie firmowej domeny internetowej tak, aby wskazywała domenę usługi Traffic Manager | Microsoft Azure"
   description="Ten artykuł pomoże Ci skonfigurować nazwę domeny firmowej w taki sposób, aby wskazywała nazwę domeny usługi Traffic Manager."
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="joaoma" />

# Ustawianie firmowej domeny internetowej tak, aby wskazywała domenę usługi Azure Traffic Manager

Aby nazwa domeny firmowej wskazywała nazwę domeny usługi Traffic Manager, należy zmodyfikować rekord zasobu DNS na internetowym serwerze DNS w celu używania typu rekordu CNAME, który mapuje nazwę domeny firmowej na nazwę domeny profilu usługi Traffic Manager. Nazwa domeny usługi Traffic Manager jest wyświetlana w sekcji **Ogólne** na stronie konfiguracji profilu usługi Traffic Manager.

Aby na przykład nazwa domeny firmowej www.contoso.com wskazywała nazwę domeny usługi Traffic Manager contoso.trafficmanager.net, należy zaktualizować rekord zasobu DNS na następujący:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Wszystkie żądania ruchu do domeny *www.contoso.com* będą teraz kierowane do domeny *contoso.trafficmanager.net*.

>[AZURE.IMPORTANT] Domena drugiego poziomu, na przykład *contoso.com*, nie może wskazywać domeny usługi Traffic Manager. Jest to ograniczenie protokołu DNS, który nie zezwala na rekordy CNAME dla nazw domen drugiego poziomu.

## Następne kroki

[Metody routingu w usłudze Traffic Manager](traffic-manager-routing-methods.md)

[Traffic Manager — wyłączanie, włączanie lub usuwanie profilu](disable-enable-or-delete-a-profile.md)

[Traffic Manager — wyłączanie lub włączanie punktu końcowego](disable-or-enable-an-endpoint.md)



<!--HONumber=Jun16_HO2-->


