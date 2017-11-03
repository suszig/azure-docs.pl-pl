---
title: "Omówienie modelu programowania sieci szkieletowej usług | Dokumentacja firmy Microsoft"
description: "Sieć szkieletowa usług oferuje dwie struktury umożliwiający tworzenie usług: framework aktora i w ramach usługi. Oferują one kompromisy różne prostoty i kontroli."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: vturecek
ms.openlocfilehash: 98e75e11b54aea677b8ca1b6f303a48190c6fce8
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="service-fabric-programming-model-overview"></a>Omówienie modelu programowania sieci szkieletowej usług
Sieć szkieletowa usług oferuje wiele sposobów, aby zapisać i zarządzania usługami. Usługi mogą być w pełni korzystać z funkcji platformy i struktury aplikacji przy użyciu interfejsów API usługi sieci szkieletowej. Można także skompilowany program wykonywalny napisane w dowolnym języku lub kodu uruchamianego w kontenerze, po prostu hostowanych w klastrze usługi sieć szkieletowa usług.

## <a name="guest-executables"></a>Pliki wykonywalne gościa
A [pliku wykonywalnego gościa](service-fabric-deploy-existing-app.md) jest istniejące, dowolnego pliku wykonywalnego (zapisany w dowolnym języku) może zostać uruchomiony jako usługa w aplikacji. Pliki wykonywalne gościa należy wywoływać bezpośrednio interfejsy API zestawu SDK sieci szkieletowej usług. Jednak one nadal korzystać z funkcji oferuje platformie, takich jak usługi odnajdywania, niestandardowe kondycji i obciążenia raportowania przez wywoływanie interfejsów API REST udostępnianych przez usługi sieć szkieletowa usług. Mają one również pełny cykl życia pomocy technicznej.

Wprowadzenie do plików wykonywalnych gościa przez wdrożenie pierwszego [aplikacja wykonywalna gościa](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Kontenery
Domyślnie usługi sieć szkieletowa wdraża i aktywuje usługi jako procesów. Sieć szkieletowa usług można także wdrożyć usług w [kontenery](service-fabric-containers-overview.md). Sieć szkieletowa usług obsługuje wdrażanie kontenery systemu Linux i kontenery systemu Windows w systemie Windows Server 2016. Kontener obrazów można ściągnąć z dowolnym repozytorium kontenera i wdrożone na maszynie. Istniejące aplikacje można wdrożyć jako gość exectuables, sieć szkieletowa usług bezstanowych i stanowych niezawodne usługi lub Reliable Actors w kontenerach i usług w kontenerach w tej samej aplikacji i usług w procesach, które można łączyć.

[Dowiedz się więcej o containerizing usług w systemie Windows lub Linux](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Niezawodne usługi to platforma lekki do pisania usług, które integrują się z platformy sieć szkieletowa usług i korzystać z pełnego zestawu funkcji platformy. Niezawodne usługi zapewniają minimalny zestaw interfejsów API zezwalające na środowisko uruchomieniowe usługi sieć szkieletowa usług do zarządzania cyklem życia usługi i korzystający z usług do interakcji z środowiska uruchomieniowego. Struktura aplikacji jest minimalny, umożliwiając pełną kontrolę nad opcje projektowania i implementacji i może służyć do hostowania wszelkie inne struktury aplikacji, takich jak ASP.NET Core.

Niezawodne usługi może być bezstanowych, podobny do większości platform usługi, takich jak serwery sieci web, w których każde wystąpienie usługi jest tworzony taki sam, a w zewnętrznych rozwiązania, takie jak bazy danych platformy Azure lub magazynu tabel platformy Azure jest trwały stan.

Niezawodne usługi można też stanowe, wyłącznie dla sieci szkieletowej usług, których stan jest trwały bezpośrednio w usłudze przy użyciu niezawodnych kolekcji. Stanu wprowadzone wysokiej dostępności przy użyciu replikacji i dystrybuowane za pośrednictwem partycje, wszystkie zarządzane automatycznie przez sieć szkieletowa usług.

[Dowiedz się więcej o usługach Reliable Services](service-fabric-reliable-services-introduction.md) lub Rozpoczynanie pracy przez [pisanie pierwszej usługi niezawodnego](service-fabric-reliable-services-quick-start.md).

## <a name="reliable-actors"></a>Reliable Actors
Wbudowane niezawodne usługi, framework niezawodnego aktora jest struktury aplikacji, która implementuje wzorca aktora wirtualnego, oparte na wzorcu aktora. Platforma niezawodnego aktora korzysta niezależne jednostki zasobów obliczeniowych i stan z jednowątkowego wykonywania o nazwie złośliwych użytkowników. Framework niezawodnego aktora zapewnia komunikacji wbudowanej złośliwych użytkowników i wstępnie ustawiony stan stanu trwałego i skalowalnego w poziomie konfiguracji.

Reliable Actors sam jest struktury aplikacji w oparciu niezawodne usługi, jego jest w pełni zintegrowana z platformy sieć szkieletowa usług i korzyści wynikające z pełnym zestawem funkcji oferowanych przez platformę.

[Dowiedz się więcej o Reliable Actors](service-fabric-reliable-actors-introduction.md) lub Rozpoczynanie pracy przez [pisanie pierwszej usługi niezawodnego aktora](service-fabric-reliable-actors-get-started.md)

## <a name="aspnet-core"></a>ASP.NET Core
Sieć szkieletowa usług integruje się z [platformy ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) do tworzenia usług sieci Web i interfejsu API, które mogą być dołączane jako część aplikacji. 

[Tworzenie usługi frontonu przy użyciu platformy ASP.NET Core](service-fabric-add-a-web-frontend.md)

## <a name="next-steps"></a>Następne kroki
[Omówienie sieci szkieletowej usług i kontenerów](service-fabric-containers-overview.md)

[Niezawodne usługi — omówienie](service-fabric-reliable-services-introduction.md)

[Niezawodne podmiotów — omówienie](service-fabric-reliable-actors-introduction.md)

[Sieć szkieletowa usług i ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




