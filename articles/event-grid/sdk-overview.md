---
title: "Zestawy SDK siatki zdarzeń platformy Azure"
description: "W tym artykule opisano zestawy SDK dla usługi Azure Event siatki. Te zestawy SDK zapewniają zarządzania, publikowanie i zużycia."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9c56e4c3314090ad55017d5c681a0cfd7bf5722c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Zestawy SDK siatki zdarzeń zarządzania i publikowanie

Zdarzenie siatki udostępnia zestawów SDK, które umożliwiają programowego zarządzania zasobami i publikowania zdarzeń.

## <a name="management-sdks"></a>Zestawy SDK zarządzania

Zarządzanie zestawów SDK Włącz tworzenie, aktualizowanie i usuwanie zdarzeń siatki tematów i subskrypcji. Obecnie dostępne są następujące zestawy SDK:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="publish-sdks"></a>Publikowanie zestawów SDK

Publikuj zestawy SDK umożliwiają post zdarzenia do tematów przez obsługę uwierzytelniania, tworzące zdarzenia i asynchronicznego ogłaszania określony punkt końcowy. Obecnie dostępne są następujące zestawy SDK:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Kolejne kroki

* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [co to jest zdarzenie siatki?](overview.md)
* Zdarzenie siatki poleceń w wiersza polecenia platformy Azure, zobacz [interfejsu wiersza polecenia Azure](/cli/azure/eventgrid).
* Dla polecenia siatki zdarzeń w programie PowerShell, zobacz [PowerShell](/powershell/module/azurerm.eventgrid).