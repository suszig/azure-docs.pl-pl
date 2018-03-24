---
title: 'Rozwiązywanie problemów: Utworzyć i nawiązywanie z obszaru roboczego usługi Machine Learning | Dokumentacja firmy Microsoft'
description: Rozwiązania typowych problemów z tworzeniem i nawiązywania połączenia z obszaru roboczego uczenia maszynowego Azure
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 5c265b14a88e993512811de365f1ba51f7ba6028
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Podręcznik rozwiązywania problemów: tworzenie obszaru roboczego usługi Machine Learning i nawiązywanie połączenia z nim
Ten przewodnik zawiera temat rozwiązań dla niektórych często spotykanych problemów podczas konfigurowania obszarów roboczych usługi Azure Machine Learning.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Właściciel obszaru roboczego
Aby otworzyć obszar roboczy usługi Machine Learning Studio, użytkownik musi zalogować się Account Microsoft użyty do utworzenia obszaru roboczego lub konieczność odbierania zaproszenia od właściciela do obszaru roboczego. W portalu Azure można zarządzać obszaru roboczego, który obejmuje możliwość konfigurowania dostępu.

Aby uzyskać więcej informacji na temat zarządzania obszaru roboczego, zobacz [Zarządzanie obszarem roboczym usługi Azure Machine Learning].

[Zarządzanie obszarem roboczym usługi Azure Machine Learning]: manage-workspace.md

## <a name="allowed-regions"></a>Dozwolonych regionów
Usługa Machine Learning jest obecnie dostępna w ograniczonej liczbie regionów. Jeśli subskrypcja nie zawiera jeden z tych regionów, zobaczysz komunikat o błędzie, "Masz żadnych subskrypcji w dozwolonych regionów."

Żądanie dodania obszaru do subskrypcji, Utwórz nowe żądanie pomocy technicznej firmy Microsoft z portalu Azure, wybierz **rozliczeń** jako typ problemu i postępuj zgodnie z monitami, aby przesłać żądanie.

## <a name="storage-account"></a>Konto magazynu
Usługa Machine Learning wymaga konta magazynu do przechowywania danych. Można użyć istniejącego konta magazynu, lub można utworzyć nowe konto magazynu, podczas tworzenia nowego obszaru roboczego uczenia maszynowego (jeśli istnieje limit przydziału, aby utworzyć nowe konto magazynu).

Po utworzeniu nowego obszaru roboczego uczenia maszynowego można logowania się do usługi Machine Learning Studio za pomocą konta Microsoft, który został użyty do utworzenia obszaru roboczego. Jeśli wystąpią komunikat o błędzie, "Obszaru roboczego nie została odnaleziona" (podobnie jak w poniższym zrzucie ekranu), aby usunąć pliki cookie przeglądarki użyj następujące kroki.

![Nie można odnaleźć obszaru roboczego][screen3]

**Aby usunąć pliki cookie przeglądarki**

1. Jeśli używasz programu Internet Explorer, kliknij przycisk **narzędzia** przycisk w prawym górnym rogu i wybierz **Opcje internetowe**.  

![Opcje internetowe][screen4]

2. W obszarze **ogólne** , kliknij pozycję **usunąć...**

![Karta Ogólne][screen5]

3. W **usuwanie historii przeglądania** okna dialogowego upewnij się, **pliki cookie i dane witryn sieci Web** jest zaznaczone, a następnie kliknij przycisk **usunąć**.

![Usuń pliki cookie][screen6]

Po usunięciu plików cookie, uruchom ponownie przeglądarkę, a następnie przejdź do [Microsoft Azure Machine Learning](https://studio.azureml.net) strony. Po wyświetleniu monitu o nazwę użytkownika i hasło, wprowadź konto Microsoft, którego użyto do utworzenia obszaru roboczego.

## <a name="comments"></a>Komentarze

Naszym celem jest zapewnienie środowiska usługi Machine Learning jako bezproblemowe, jak to możliwe. Opublikuj uwagi i problemy z [forum usługi Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) w celu ułatwienia obsługi można lepiej.

[screen1]:media/troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/troubleshooting-creating-ml-workspace/screen6.png
