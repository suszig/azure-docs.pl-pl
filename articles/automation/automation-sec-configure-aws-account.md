---
title: "Konfigurowanie uwierzytelniania w usłudze Amazon Web Services | Microsoft Docs"
description: "W tym artykule opisano sposób tworzenia i sprawdzania poprawności poświadczeń w usłudze AWS dla elementów Runbook w usłudze Azure Automation zarządzającej zasobami usługi AWS."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: uwierzytelnianie aws, konfigurowanie aws
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 00b217a4cddac0a893564db27ffb4f460973c246
ms.openlocfilehash: eee65672b3a9615afe2850b0cbc6daa275fc04ec


---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Uwierzytelnianie elementów Runbook w usłudze Amazon Web Services
Typowe zadania dotyczące zasobów w usłudze Amazon Web Services (AWS) można zautomatyzować za pomocą elementów Runbook usługi Azure Automation.  Wiele zadań w usłudze AWS można zautomatyzować przy użyciu elementów Runbook usługi Automation, podobnie jak za pomocą zasobów platformy Azure.  Wymagane są jedynie dwa elementy:

* Subskrypcja AWS i zestaw poświadczeń.  W szczególności klucz dostępu AWS i klucz tajny.  Więcej informacji można znaleźć w artykule [Using AWS Credentials](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) (Korzystanie z poświadczeń usługi AWS).
* Subskrypcja platformy Azure i konto w usłudze Automation.  Więcej informacji na temat konfigurowania konta usługi Azure Automation można znaleźć w artykule [Authenticate runbooks with an Azure Run As account](automation-sec-configure-azure-runas-account.md) (Uwierzytelnianie elementów Runbook za pomocą konta platformy Azure Uruchom jako).  

Aby wykonać uwierzytelnienie w usłudze AWS, należy określić zestaw poświadczeń usługi AWS do uwierzytelniania elementów Runbook uruchamianych z poziomu usługi Azure Automation. Jeśli masz już konto w usłudze Automation i chcesz go użyć do uwierzytelnienia w usłudze AWS, możesz wykonać kroki opisane w poniższej sekcji.  Jeśli chcesz mieć specjalne konto dla elementów Runbook odwołujących się do zasobów AWS, najpierw utwórz nowe [konto usługi Automation Uruchom jako](automation-sec-configure-azure-runas-account.md) (pomiń opcję tworzenia nazwy głównej usługi), a następnie wykonaj poniższe kroki.

## <a name="configure-automation-account"></a>Skonfiguruj konto usługi Automation
Aby usługa Azure Automation mogła się komunikować z usługą AWS, należy najpierw pobrać poświadczenia usługi AWS i przechowywać je jako zasoby w usłudze Azure Automation.  Wykonaj następujące czynności opisane w dokumencie usługi AWS [Zarządzanie kluczami dostępu konta usługi AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html), aby utworzyć klucz dostępu i skopiować **identyfikator klucza dostępu** i **tajny klucz dostępu** (ewentualnie pobierz plik klucza, aby go bezpiecznie przechować w innym miejscu).

Po utworzeniu i skopiowaniu kluczy zabezpieczeń usługi AWS należy utworzyć zasób poświadczeń przy użyciu konta usługi Azure Automation, aby bezpiecznie je przechowywać i odwoływać się do nich z poziomu elementów Runbook.  Wykonaj kroki opisane w sekcji **Creating a new credential asset** (Tworzenie nowego elementu zawartości poświadczenia) artykułu [Credential assets in Azure Automation](automation-credentials.md#creating-a-new-credential-asset) (Zasoby poświadczeń w usłudze Azure Automation), a następnie wprowadź następujące informacje:

1. W polu **Nazwa** wprowadź **AWScred** lub wartość odpowiadającą Twoim standardom nazewnictwa.  
2. W polu **Nazwa użytkownika** wpisz swój **identyfikator dostępu** oraz **tajny klucz dostępu** w polach **Hasło** i **Potwierdź hasło**.   

## <a name="next-steps"></a>Następne kroki
* Zapoznaj się z artykułem [Automating deployment of a VM in Amazon Web Services](automation-scenario-aws-deployment.md) (Zautomatyzowane wdrażanie maszyn wirtualnych w usłudze Amazon Web Services), aby dowiedzieć się, jak utworzyć elementy Runbook w celu automatyzacji zadań w usłudze AWS.



<!--HONumber=Nov16_HO2-->


