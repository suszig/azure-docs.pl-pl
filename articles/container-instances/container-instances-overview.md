---
title: "Omówienie usługi Azure Container Instances | Azure Docs"
description: "Informacje dotyczące usługi Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6e614f1120b3dc54871b393ac0a2703c21b30ae8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-container-instances"></a>Azure Container Instances

Kontenery szybko stają się preferowanym sposobem tworzenia pakietów aplikacji w chmurze, ich wdrażania oraz zarządzania nimi. Usługa Azure Container Instances to najszybszy i najprostszy sposób uruchomienia kontenera na platformie Azure, bez konieczności aprowizowania żadnych maszyn wirtualnych ani korzystania z usługi wyższego poziomu.

Usługa Azure Container Instances to doskonałe rozwiązanie dla wszystkich scenariuszy, które może działać w kontenerach izolowanych, w tym w przypadku prostych aplikacji, automatyzacji zadań i zadań kompilacji. W przypadku scenariuszy, w których potrzebujesz pełnej aranżacji kontenerów, w tym opcji odnajdowania usług w wielu kontenerach, automatycznego skalowania i skoordynowanych uaktualnień aplikacji, zalecamy skorzystanie z usługi [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

## <a name="fast-startup-times"></a>Krótki czas uruchamiania

Kontenery oferują znaczące korzyści związane z uruchamianiem w porównaniu do maszyn wirtualnych. Usługa Azure Container Instances umożliwia uruchamianie kontenera na platformie Azure w ciągu kilku sekund bez konieczności aprowizowania maszyn wirtualnych i zarządzania nimi.

## <a name="hypervisor-level-security"></a>Zabezpieczenia na poziomie funkcji hypervisor

W przeszłości kontenery oferowały zarządzanie zasobami i izolację zależności aplikacji, ale nie były wystarczająco odporne na użycie wielu obcych dzierżaw. Dzięki usłudze Azure Container Instances aplikacja jest izolowana w kontenerze w takim samym stopniu, w jakim byłaby w maszynie wirtualnej.

## <a name="custom-sizes"></a>Rozmiary niestandardowe

Kontenery są przeważnie optymalizowane do uruchamiania tylko jednej aplikacji, ale szczegółowe potrzeby poszczególnych aplikacji mogą się bardzo różnić. W usłudze Azure Container Instances możesz zażądać dokładnie tylu rdzeni i pamięci, ile potrzebujesz. Opłaty są naliczane zgodnie z żądaniami i za sekundę, dlatego można skutecznie zoptymalizować wydatki o oparciu o potrzeby.

## <a name="public-ip-connectivity"></a>Łączność przy użyciu publicznych adresów IP

Dzięki usłudze Azure Container Instances można uwidaczniać kontenery bezpośrednio w Internecie za pomocą publicznego adresu IP. W przyszłości zamierzamy rozszerzyć nasze możliwości sieciowe, aby uwzględnić integrację z sieciami wirtualnymi, modułami równoważenia obciążenia i innymi podstawowymi elementami infrastruktury sieci platformy Azure.

## <a name="persistent-storage"></a>Magazyn trwały

Aby umożliwić pobieranie i utrwalanie stanu przy użyciu usługi Azure Container Instances, firma Microsoft oferuje bezpośrednie instalowanie udziałów plików platformy Azure.

## <a name="linux-and-windows-containers"></a>Kontenery systemów Linux i Windows

Usługa Azure Container Instances umożliwia planowanie kontenerów systemów Windows i Linux przy użyciu tego samego interfejsu API. Wystarczy wskazać typ podstawowego systemu operacyjnego — wszystkie inne elementy są identyczne.

## <a name="co-scheduled-groups"></a>Grupy planowane wspólnie

Usługa Azure Container Instances obsługuje planowanie grup wielu kontenerów, które współużytkują maszynę hosta, sieć lokalną, magazyn i cykl życia. Dzięki temu można łączyć główną aplikację z innymi, które pełnią rolę pomocniczą, takimi jak rejestrowanie.

## <a name="next-steps"></a>Następne kroki

Spróbuj wdrożyć kontener na platformie Azure za pomocą jednego polecenia, korzystając z naszego [przewodnika Szybki Start](container-instances-quickstart.md).