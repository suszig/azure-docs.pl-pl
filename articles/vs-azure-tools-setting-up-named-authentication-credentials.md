---
title: "Konfigurowanie poświadczeń uwierzytelniania o nazwie | Dokumentacja firmy Microsoft"
description: "Informacje o podanie poświadczeń, które Visual Studio można użyć do uwierzytelniania żądań do usługi Azure, co umożliwia publikowanie aplikacji na platformie Azure w programie Visual Studio lub monitorować istniejącą usługę w chmurze."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 953b1aa459ddf5b7be00b9d32432e6dda97143e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-named-authentication-credentials"></a>Konfigurowanie poświadczeń uwierzytelniania o nazwie
Aby opublikować aplikację na platformie Azure w programie Visual Studio lub do monitorowania istniejącej usługi chmury, musisz podać poświadczenia, które Visual Studio można użyć do uwierzytelniania żądań do usługi Azure. Istnieje kilka obszarów w programie Visual Studio, w którym można logowania się do tych poświadczenia. Na przykład z poziomu Eksploratora serwera, można otworzyć z menu skrótów **Azure** a następnie wybierz węzeł **nawiązywanie połączenia z subskrypcją platformy Microsoft Azure**. Po zalogowaniu, informacji o subskrypcji, który został skojarzony z Twoim kontem platformy Azure jest dostępna w programie Visual Studio. Nie ma nic więcej trzeba wykonać.

Narzędzia platformy Azure obsługuje również starsze sposób podawania poświadczeń: przy użyciu subskrypcji (plik .publishsettings). W tym artykule opisano metody nadal jest obsługiwany w 2.2 zestawu SDK platformy Azure.

Następujące elementy są wymagane do uwierzytelniania na platformie Azure:

* Identyfikator subskrypcji
* Prawidłowy certyfikat X.509 v3

> [!NOTE]
> Długość klucza certyfikat X.509 v3 musi być co najmniej 2048 bitów. Azure odrzuca dowolny certyfikat, który nie spełnia tego wymagania lub nie jest prawidłowa.
>
>

Visual Studio będzie korzystać identyfikator subskrypcji oraz dane certyfikatu jako poświadczeń. Odpowiednie poświadczenia odwołuje się plik subskrypcji (pliku .publishsettings,), który zawiera klucz publiczny certyfikatu. Plik subskrypcji może zawierać poświadczeń dla więcej niż jedną subskrypcję.

Można edytować informacji o subskrypcji z **nową subskrypcję** lub **edytować subskrypcji** okno dialogowe, zgodnie z objaśnieniem w dalszej części tego artykułu.

Jeśli chcesz samodzielnie utworzyć certyfikat mogą odwoływać się do instrukcji w [tworzenie i przekazywanie certyfikatu zarządzania platformy Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) , a następnie ręcznie przekazać certyfikat do [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Te poświadczenia, które Visual Studio wymaga, aby zarządzać usługi w chmurze nie są tymi samymi poświadczeniami, które są wymagane do uwierzytelniania żądań dla usług Azure storage.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>Importuj, konfigurowanie lub edytowanie poświadczeń uwierzytelniania w programie Visual Studio

1. W Eksploratorze serwera Otwórz menu skrótów **Azure** a następnie wybierz węzeł **zarządzanie i subskrypcje filtru**.
2. Wybierz **certyfikaty** karcie, a następnie użyj jednej z następujących metod:

    * Wybierz **importu** otworzyć **importu subskrypcji platformy Microsoft Azure** okno dialogowe. , Możesz pobrać plik subskrypcje aktualnie załadowanych subskrypcji, przejdź do lokalizacji pobierania, a następnie zaimportuj go do użytku podczas uwierzytelniania.
    * Wybierz **nowy** otworzyć **nową subskrypcję** okno dialogowe. Istnieje można skonfigurować nowej subskrypcji do użytku podczas uwierzytelniania.
    * Wybierz **Edytuj** (po wybraniu aktywnych subskrypcji) można otworzyć **edytować subskrypcji** okno dialogowe. Istnieje można edytować istniejącej subskrypcji do użytku podczas uwierzytelniania. 

W poniższej procedurze przyjęto, że **nową subskrypcję** jest otwarte okno dialogowe.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Aby skonfigurować poświadczenia uwierzytelniania w programie Visual Studio
1. W **wybierz istniejący certyfikat na potrzeby uwierzytelniania** wybierz certyfikat.
2. Wybierz **skopiować pełną ścieżkę** łącza. Ścieżka certyfikatu (pliku cer) jest kopiowany do Schowka.

   > [!IMPORTANT]
   > Aby opublikować aplikację Azure w programie Visual Studio, należy przesłać ten certyfikat do [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   >
   >
3. Aby przekazać certyfikat do portalu Azure:

   a. Otwórz [portal Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   
   b. Jeśli zostanie wyświetlony monit, zaloguj się do portalu, a następnie przejdź do **ustawienia** > **certyfikaty zarządzania**.
   
   c. W **certyfikaty zarządzania** okienku wybierz **przekazać**.
   
   d. Wybierz subskrypcję platformy Azure, Wklej pełną ścieżkę pliku .cer, który został właśnie utworzony, a następnie wybierz **przekazać**.

## <a name="next-steps"></a>Następne kroki
* [Ogólne omówienie aplikacji sieci Web](https://docs.microsoft.com/azure/app-service/)
* [Wdrażanie aplikacji w usłudze Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-deploy-local-git) 
* [Wdrażanie zadań WebJob za pomocą programu Visual Studio](https://docs.microsoft.com/en-us/azure/app-service/websites-dotnet-deploy-webjobs)
* [Tworzenie i wdrażanie usługi w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)