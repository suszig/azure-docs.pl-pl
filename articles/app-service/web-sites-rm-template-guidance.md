---
title: "Wskazówki dotyczące wdrażania aplikacji sieci web platformy Azure za pomocą szablonów | Dokumentacja firmy Microsoft"
description: "Zalecenia dotyczące tworzenia szablonów usługi Azure Resource Manager do wdrażania aplikacji sieci web."
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: tomfitz
ms.openlocfilehash: eff0b0e6258217fa8fbf7f15606f98d70fecd7c5
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="guidance-on-deploying-web-apps-with-azure-resource-manager-templates"></a>Wskazówki dotyczące wdrażania aplikacji sieci web za pomocą szablonów usługi Azure Resource Manager

Ten artykuł zawiera zalecenia dotyczące tworzenia szablonów usługi Azure Resource Manager, aby wdrożyć rozwiązania aplikacji usługi. Zalecenia te mogą pomóc w uniknięciu typowych problemów.

## <a name="define-dependencies"></a>Definiowanie zależności

Definiowanie zależności dla aplikacji sieci Web wymaga zrozumienia interakcje między zasobami w aplikacji sieci Web. Jeśli określisz zależności w niepoprawna kolejność może powodować błędy wdrażania lub utworzyć wyścigu zatrzymuje wdrożenia.

> [!WARNING]
> Jeśli w szablonie rozszerzeniem lokacji MS Deploy, wszystkie zasoby konfiguracji musi być ustawiona jako zależy od zasobu MS Deploy. Zmiany konfiguracji powodują lokacji ponownie uruchomić asynchronicznie. Dzięki konfiguracji zasoby zależne MS Deploy, należy zwrócić uwagę MS Deploy zakończy się przed ponownym uruchomieniem lokacji. Bez tych zależności lokacji może być ponownie podczas procesu wdrażania MS Deploy. Przykład szablonu, zobacz [szablonu Wordpress z zależnością wdrażania Web](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

Na poniższej ilustracji przedstawiono kolejność zależności dla różnych zasobów usługi aplikacji.

![Zależności aplikacji sieci Web](media/web-sites-rm-template-guidance/web-dependencies.png)

Wdrażania zasobów w następującej kolejności:

**Warstwy 1**
* Plan usługi App Service
* Powiązane zasoby, takie jak bazy danych lub kont magazynu

**Warstwy 2**
* Sieci Web app — zależy od planu usługi aplikacji
* Usługa Application Insights, które odnoszą się do farmy serwerów - zależy od planu usługi aplikacji

**Warstwy 3**
* Kontroli źródła — zależy od aplikacji sieci web
* Rozszerzenie lokacji MSDeploy - zależy od aplikacji sieci web
* Usługa Application Insights, które odnoszą się do farmy serwerów - zależy od aplikacji sieci web

**Warstwy 4**
* Certyfikat usługi aplikacji — zależna od kontroli źródła lub MSDeploy, jeśli spełniony jest obecny; w przeciwnym razie wartość zależy od aplikacji sieci web
* Ustawienia konfiguracji (parametry połączenia, wartości konfiguracji sieci web, ustawienia aplikacji) - zależy od kontroli źródła lub MSDeploy jeśli spełniony jest obecny; w przeciwnym razie wartość zależy od aplikacji sieci web

**Warstwa 5**
* Host name powiązania — zależy od certyfikatu, jeśli istnieje; w przeciwnym razie wyższego poziomu zasobów
* Lokacji rozszerzenia — zależy od ustawień konfiguracji, jeśli występuje; w przeciwnym razie wyższego poziomu zasobów

Zazwyczaj rozwiązania zawiera tylko niektóre z tych zasobów i warstw. Brak warstw mapy niższe zasobów do następnego wyższego poziomu.

W poniższym przykładzie przedstawiono częścią szablonu. Wartość konfiguracji ciągu połączenia jest zależna od rozszerzenia MSDeploy. Rozszerzenie MSDeploy zależy od aplikacji sieci web i bazy danych.

```json
{
    "name": "[parameters('name')]",
    "type": "Microsoft.Web/sites",
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'))]",
            "[concat('SuccessBricks.ClearDB/databases/', parameters('databaseName'))]"
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

## <a name="find-information-about-msdeploy-errors"></a>Informacje o błędach MSDeploy

Jeśli szablon usługi Resource Manager używa MSDeploy, komunikaty o błędach wdrożenia może być trudne do zrozumienia. Aby uzyskać więcej informacji, po wdrożeniu nie powiodło się, spróbuj wykonać następujące kroki:

1. Przejdź do witryny [konsoli Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console).
2. Przejdź do folderu na `D:\home\LogFiles\SiteExtensions\MSDeploy`.
3. Wyszukaj *appManagerStatus.xml* i *appManagerLog.xml* plików. Pierwszy plik rejestruje stan. Drugi plik rejestruje informacje o tym błędzie. Jeśli ten błąd nie jest jasne, do Ciebie, może zawierać podczas pytania o pomoc na forum.

## <a name="unique-web-app-name"></a>Nazwa aplikacji sieci web unikatowy

Nazwa aplikacji sieci web musi być globalnie unikatowa. Można użyć konwencji nazewnictwa może być unikatowy, lub można użyć [funkcja uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) pomagające generowania unikatową nazwę.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="next-steps"></a>Kolejne kroki

* Samouczek dotyczący wdrażania aplikacji sieci web za pomocą szablonu, zobacz [udostępniania i wdrażanie mikrousług przewidywalnego na platformie Azure](app-service-deploy-complex-application-predictably.md).