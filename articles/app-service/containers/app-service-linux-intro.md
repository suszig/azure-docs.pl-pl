---
title: "Wprowadzenie do usługi App Service w systemie Linux | Dokumentacja firmy Microsoft"
description: "Więcej informacji o usłudze Azure App Service w systemie Linux."
keywords: "Usługa aplikacji Azure, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/16/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 89cb7dc488da42724f212d13f8550064ff8b9188
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Wprowadzenie do usługi Azure App Service w systemie Linux

[Sieci Web aplikacji](../app-service-web-overview.md) jest w pełni zarządzana platforma obliczeniowa zoptymalizowana pod kątem obsługi aplikacji sieci web i witryn sieci Web. Klienci mogą użyć usługi aplikacji w systemie Linux do aplikacji sieci web hosta natywnie w systemie Linux dla stosy obsługiwanej aplikacji. Poniższe sekcje Wyświetla stosy aplikacji, które są obecnie obsługiwane.

## <a name="languages"></a>Języki

Usługa aplikacji w systemie Linux obsługuje szereg wbudowanych obrazów w celu zwiększenia produktywności deweloperów. Jeśli aplikacja wymaga środowiska uruchomieniowego nie jest obsługiwana w obrazach wbudowanych, istnieją instrukcje dotyczące sposobu [utworzyć własny obraz Docker](tutorial-custom-docker-image.md) do wdrożenia aplikacji sieci Web dla kontenerów.

| Język | Obsługiwane wersje |
|---|---|
| Node.js | 4.4, 4.5, 6.2, 6.6, 6.9-6.11, 8.0, 8.1 |
| PHP | 5.6, 7.0 |
| .NET Core | 1.0, 1.1 |
| Ruby | 2.3 |

## <a name="deployments"></a>Wdrożenia

* FTP
* Lokalne Git
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Środowiska przejściowe
* [Kontener Azure rejestru](https://docs.microsoft.com/azure/container-registry/container-registry-intro) i DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>Konsola, publikowanie i debugowania

* Środowisk
* Wdrożenia
* Podstawowe konsoli
* SSH

## <a name="scaling"></a>Skalowanie

* Klientów można skalować aplikacji sieci web górę i w dół, zmieniając warstwę z ich [planu usługi aplikacji](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview?toc=%2fazure%2fapp-service-web%2ftoc.json)

## <a name="locations"></a>Lokalizacje

Sprawdź [pulpitu nawigacyjnego platformy Azure stan](https://azure.microsoft.com/status).

## <a name="limitations"></a>Ograniczenia

Azure portal zawiera tylko te funkcje, które obecnie działają dla aplikacji sieci Web dla kontenerów. Jak możemy włączyć więcej funkcji, będzie widoczny w portalu.

Niektóre funkcje, takie jak integracji sieci wirtualnej, Azure Active Directory/uwierzytelniania innej firmy lub Kudu rozszerzenia lokacji, nie są jeszcze dostępne. Gdy te funkcje są dostępne, aktualizujemy nasze dokumentację i blog o zmianach.

Usługa aplikacji w systemie Linux jest obsługiwana tylko z [Basic i Standard](https://azure.microsoft.com/pricing/details/app-service/plans/) planów usługi aplikacji, a nie ma [wolne lub Shared](https://azure.microsoft.com/pricing/details/app-service/plans/) warstwy. Poniżej przedstawiono również ważne ograniczenia dla aplikacji usługi w systemie Linux:

* Nie można utworzyć aplikacji sieci Web dla kontenerów w planie usługi aplikacji już hosting aplikacji sieci Web z systemem innym niż Linux.
* Podczas tworzenia aplikacji sieci Web dla kontenerów w grupie zasobów zawierające aplikacje sieci Web z systemem innym niż Linux, należy utworzyć plan usługi aplikacji w regionie innym niż istniejący plan usługi aplikacji.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Aplikacji nie powiedzie się lub chcesz sprawdzić rejestrowania z aplikacji, sprawdź dzienniki w katalogu LogFiles Docker. Za pomocą witryny SCM, lub za pośrednictwem protokołu FTP można uzyskać dostępu do tego katalogu.
W dzienniku `stdout` i `stderr` z Twojej kontenera, musisz włączyć **rejestrowania kontenera Docker** w obszarze **dzienników diagnostycznych**.

![Włączanie rejestrowania][2]

![Aby wyświetlić dzienniki Docker przy użyciu Kudu][1]

Można uzyskać dostępu do lokacji SCM z **zaawansowane narzędzia** w **narzędzi programistycznych** menu.

## <a name="next-steps"></a>Następne kroki

Zobacz poniższe łącza, aby rozpocząć korzystanie z usługi aplikacji w systemie Linux. Pytania i uwagi można umieścić na [naszym forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Sposób użycia niestandardowego obrazu Docker dla aplikacji sieci Web dla kontenerów](quickstart-custom-docker-image.md)
* [W usłudze Azure App Service w systemie Linux przy użyciu platformy .NET Core](quickstart-dotnetcore.md)
* [W usłudze Azure App Service w systemie Linux przy użyciu Ruby](quickstart-ruby.md)
* [Usługa aplikacji Azure aplikacji sieci Web dla kontenerów — często zadawane pytania](app-service-linux-faq.md)
* [Obsługa protokołu SSH dla usługi Azure App Service w systemie Linux](app-service-linux-ssh-support.md)
* [Konfigurowanie środowisk w usłudze Azure App Service przejściowych](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Docker Centrum ciągłego wdrażania aplikacji sieci Web dla kontenerów](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
