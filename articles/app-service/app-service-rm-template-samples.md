---
title: "Przykłady szablonów usługi Azure Resource Manager — App Service | Microsoft Docs"
description: "Przykłady szablonów usługi Azure Resource Manager — App Service"
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
editor: ggailey777
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 02/26/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: e28a27b9a00164fcbba6eb5d3e5435548486ffa4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="azure-resource-manager-templates-for-azure-web-apps"></a>Szablony usługi Azure Resource Manager dla funkcji Azure Web Apps

Poniższa tabela zawiera linki do szablonów usługi Azure Resource Manager. Aby zapoznać się z zaleceniami dotyczącymi unikania typowych błędów podczas tworzenia szablonów aplikacji internetowych, zobacz [Guidance on deploying web apps with Azure Resource Manager templates (Wskazówki dotyczące wdrażania aplikacji internetowych przy użyciu szablonów usługi Azure Resource Manager)](web-sites-rm-template-guidance.md).

| | |
|-|-|
|**Wdrażanie aplikacji internetowej**||
| [Aplikacja internetowa połączona z repozytorium GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Wdraża aplikację internetową platformy Azure, która ściąga kod z usługi GitHub. |
| [Aplikacja internetowa z niestandardowymi miejscami wdrożenia](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Wdraża aplikację internetową platformy Azure z niestandardowymi środowiskami/miejscami wdrożenia. |
|**Konfigurowanie aplikacji internetowej**||
| [Certyfikat aplikacji internetowej z magazynu Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Wdraża certyfikat aplikacji internetowej platformy Azure z wpisu tajnego magazynu Key Vault i używa go na potrzeby powiązania SSL. |
| [Aplikacja internetowa z domeną niestandardową](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Wdraża aplikację internetową platformy Azure z niestandardową nazwą hosta. |
| [Aplikacja internetowa z protokołem SSL i domeną niestandardową](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Wdraża aplikację internetową platformy Azure z niestandardową nazwą hosta, a następnie pobiera certyfikat aplikacji internetowej z magazynu Key Vault na potrzeby powiązania SSL. |
| [Aplikacja internetowa z rozszerzeniem Golang](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Wdraża aplikację internetową platformy Azure z rozszerzeniem lokacji Golang, co pozwala na uruchamianie aplikacji internetowych utworzonych w usłudze Golang na platformie Azure. |
| [Aplikacja internetowa z rozwiązaniami Java 8 i Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Wdraża aplikację internetową platformy Azure z włączonym językiem Java 8 i programem Tomcat 8, co pozwala na uruchamianie aplikacji Java na platformie Azure. |
|**Aplikacja internetowa systemu Linux**||
| [Aplikacja internetowa w systemie Linux z rozwiązaniem MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Wdraża aplikację internetową platformy Azure w systemie Linux przy użyciu bazy danych platformy Azure na potrzeby rozwiązania MySQL. |
| [Aplikacja internetowa w systemie Linux z rozwiązaniem PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Wdraża aplikację internetową platformy Azure w systemie Linux przy użyciu bazy danych platformy Azure na potrzeby rozwiązania PostgreSQL. |
|**Aplikacja internetowa z połączonymi zasobami**||
| [Aplikacja internetowa z rozwiązaniem MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Wdraża aplikację internetową platformy Azure w systemie Windows przy użyciu bazy danych platformy Azure dla języka MySQL. |
| [Aplikacja internetowa z rozwiązaniem PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Wdraża aplikację internetową platformy Azure w systemie Windows przy użyciu bazy danych platformy Azure na potrzeby rozwiązania PostgreSQL. |
| [Aplikacja internetowa z bazą danych SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Wdraża aplikację internetową i bazę danych SQL Database na poziomie usługi „Podstawowa”. |
| [Aplikacja internetowa z połączeniem z usługą Blob Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Wdraża aplikację internetową platformy Azure przy użyciu parametrów połączenia magazynu obiektów blob, co pozwala na korzystanie z usługi Azure Blob Storage w aplikacji internetowej. |
| [Aplikacja internetowa z pamięcią podręczną Redis Cache](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Wdraża aplikację internetową platformy Azure z pamięcią podręczną Redis Cache. |
|**Środowisko usługi App Service**||
| [Tworzenie środowiska App Service Environment w wersji 2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Tworzy środowisko App Service Environment w wersji 2 w sieci wirtualnej. |
| [Tworzenie środowiska App Service Environment w wersji 2 przy użyciu adresu wewnętrznego modułu równoważenia obciążenia](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Tworzy środowisko App Service Environment w wersji 2 w sieci wirtualnej przy użyciu prywatnego adresu wewnętrznego modułu równoważenia obciążenia. |
| [Konfigurowanie domyślnego certyfikatu SSL na potrzeby środowiska ASE wewnętrznego modułu równoważenia obciążenia lub środowiska ASE wewnętrznego modułu równoważenia obciążenia w wersji 2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Konfiguruje domyślny certyfikat SSL dla środowiska App Service Environment wewnętrznego modułu równoważenia obciążenia lub środowiska App Service Environment wewnętrznego modułu równoważenia obciążenia w wersji 2 |
| | |
