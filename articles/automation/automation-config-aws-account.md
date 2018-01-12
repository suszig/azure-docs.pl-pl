---
title: "Konfigurowanie uwierzytelniania w usłudze Amazon Web Services | Microsoft Docs"
description: "W tym artykule opisano sposób tworzenia i sprawdzania poprawności poświadczeń w usłudze AWS dla elementów Runbook w usłudze Azure Automation zarządzającej zasobami usługi AWS."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: uwierzytelnianie aws, konfigurowanie aws
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 68805a6b28fc9454262cb0503daa23af93c76a7e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Uwierzytelnianie elementów Runbook w usłudze Amazon Web Services
Typowe zadania dotyczące zasobów w usłudze Amazon Web Services (AWS) można zautomatyzować za pomocą elementów Runbook usługi Azure Automation.  Wiele zadań w usłudze AWS można zautomatyzować przy użyciu elementów Runbook usługi Automation, podobnie jak za pomocą zasobów platformy Azure.  Wymagane są jedynie dwa elementy:

* Subskrypcja AWS i zestaw poświadczeń.  W szczególności klucz dostępu AWS i klucz tajny.  Więcej informacji można znaleźć w artykule [Using AWS Credentials (Korzystanie z poświadczeń usługi AWS)](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Subskrypcja platformy Azure i konto w usłudze Automation.  Aby uzyskać więcej informacji na temat konfigurowania konta usługi Azure Automation, zapoznaj się z sekcją dotyczącą [planowania uwierzytelniania](automation-offering-get-started.md#authentication-planning).  

Aby wykonać uwierzytelnienie w usłudze AWS, należy określić zestaw poświadczeń usługi AWS do uwierzytelniania elementów Runbook uruchamianych z poziomu usługi Azure Automation. Jeśli masz już konto w usłudze Automation i chcesz go użyć do uwierzytelnienia w usłudze AWS, możesz wykonać kroki opisane w poniższej sekcji.  Jeśli chcesz mieć specjalne konto dla elementów runbook odwołujących się do zasobów AWS, najpierw utwórz nowe [konto usługi Automation](automation-offering-get-started.md) (pomiń opcję tworzenia jednostki usługi), a następnie wykonaj poniższe kroki.

## <a name="configure-automation-account"></a>Skonfiguruj konto usługi Automation
Aby usługa Azure Automation mogła się komunikować z usługą AWS, musisz najpierw pobrać poświadczenia usługi AWS i przechowywać je jako zasoby w usłudze Azure Automation.  Wykonaj następujące czynności opisane w dokumencie usługi AWS [Zarządzanie kluczami dostępu konta usługi AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html), aby utworzyć klucz dostępu i skopiować **identyfikator klucza dostępu** i **tajny klucz dostępu** (ewentualnie pobierz plik klucza, aby go bezpiecznie przechować w innym miejscu).

Po utworzeniu i skopiowaniu kluczy zabezpieczeń usługi AWS musisz utworzyć zasób poświadczeń przy użyciu konta usługi Azure Automation, aby bezpiecznie je przechowywać i odwoływać się do nich z poziomu elementów Runbook.  Wykonaj kroki opisane w sekcji **To create a new credential** (Tworzenie nowego poświadczenia) artykułu [Credential assets in Azure Automation](automation-credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) (Zasoby poświadczeń w usłudze Azure Automation), a następnie wprowadź następujące informacje:

1. W polu **Nazwa** wprowadź **AWScred** lub wartość odpowiadającą Twoim standardom nazewnictwa.  
2. W polu **Nazwa użytkownika** wpisz swój **identyfikator dostępu** oraz **klucz tajny dostępu** w polach **Hasło** i **Potwierdź hasło**.   

## <a name="next-steps"></a>Kolejne kroki
* Zapoznaj się z artykułem [Automating deployment of a VM in Amazon Web Services (Zautomatyzowane wdrażanie maszyn wirtualnych w usłudze Amazon Web Services)](automation-scenario-aws-deployment.md), aby dowiedzieć się, jak utworzyć elementy runbook w celu automatyzacji zadań w usłudze AWS.

