---
title: Korzystanie z kontroli dostępu opartej na rolach w witrynie Azure Portal | Microsoft Docs
description: Rozpocznij zarządzanie dostępem przy użyciu kontroli dostępu opartej na rolach w witrynie Azure Portal. Przypisz uprawnienia do swoich zasobów za pomocą przypisań ról.
services: active-directory
documentationcenter: ''
author: kgremban
manager: femila
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/10/2016
ms.author: kgremban

---
# Zarządzanie dostępem do zasobów subskrypcji platformy Azure za pomocą przypisań ról
> [!div class="op_single_selector"]
> * [Zarządzanie dostępem użytkowników lub grup](role-based-access-control-manage-assignments.md)
> * [Zarządzanie dostępem do zasobów](role-based-access-control-configure.md)
> 
> 

Kontrola dostępu oparta na rolach (Role-Based Access Control, RBAC) na platformie Azure umożliwia precyzyjne zarządzanie dostępem dla platformy Azure. Korzystając z modelu RBAC, można udzielić użytkownikom tylko takiego dostępu, jakiego potrzebują do wykonania swoich zadań. Ten artykuł ułatwia rozpoczęcie pracy z kontrolą dostępu opartą na rolach w witrynie Azure Portal. Jeśli chcesz uzyskać więcej szczegółowych informacji na temat sposobu, w jaki RBAC ułatwia zarządzanie dostępem, zobacz [Co to jest kontrola dostępu oparta na rolach](role-based-access-control-what-is.md).

## Wyświetlanie dostępu
Z poziomu głównego bloku zasobu, grupy zasobów lub subskrypcji w witrynie [Azure Portal](https://portal.azure.com) można zobaczyć, kto ma dostęp do tego zasobu, grupy zasobów lub subskrypcji. Na przykład jeśli chcesz zobaczyć, kto ma dostęp do jednej z grup zasobów:

1. Wybierz pozycję **Grupy zasobów** na pasku nawigacyjnym po lewej stronie.  
    ![Grupy zasobów — ikona](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Wybierz nazwę grupy zasobów z bloku **Grupy zasobów**.
3. Z menu po lewej stronie wybierz opcję **Kontrola dostępu (IAM)**.  
4. W bloku Kontrola dostępu znajduje się lista wszystkich użytkowników, grup i aplikacji, którym został udzielony dostęp do grupy zasobów.  
   
    ![Blok użytkowników — dostęp dziedziczony a przypisany (zrzut ekranu)](./media/role-based-access-control-configure/view-access.png)

Należy zauważyć, że w przypadku niektórych użytkowników dostęp został **przypisany**, a w przypadku innych jest **dziedziczony**. Dostęp jest przypisywany specjalnie do grupy zasobów albo dziedziczony z przypisania do subskrypcji nadrzędnej.

> [!NOTE]
> Klasyczni administratorzy i współadministratorzy są traktowani jako właściciele subskrypcji w nowym modelu RBAC.
> 
> 

## Dodawanie dostępu
Dostęp udzielany jest w ramach zasobu, grupy zasobów lub subskrypcji, która jest zakresem przypisania roli.

1. Kliknij przycisk **Dodaj** w bloku Kontrola dostępu.  
2. W bloku **Wybierz rolę** wybierz rolę, którą chcesz przypisać.
3. Wybierz użytkownika, grupę lub aplikację w katalogu, którym chcesz udzielić dostępu. Możesz przeszukiwać katalog przy użyciu nazw wyświetlanych, adresów e-mail i identyfikatorów obiektów.  
   
    ![Blok dodawania użytkowników — zrzut ekranu wyszukiwania](./media/role-based-access-control-configure/grant-access2.png)
4. Wybierz przycisk **OK**, aby utworzyć przypisanie. Wyskakujące okienko **Dodawanie użytkownika** śledzi postęp procesu.  
    ![Pasek postępu dodawania użytkownika — zrzut ekranu](./media/role-based-access-control-configure/addinguser_popup.png)

Po pomyślnym dodaniu przypisania roli będzie ono wyświetlane w bloku **Użytkownicy**.

## Usuwanie dostępu
1. Wybierz przypisanie roli w bloku Kontrola dostępu.
2. Wybierz przycisk **Usuń** w bloku szczegółów przypisania.  
3. Wybierz opcję **Tak**, aby potwierdzić usunięcie.  
    ![Blok Użytkownicy — usuwanie z roli (zrzut ekranu)](./media/role-based-access-control-configure/remove-access1.png)

Przypisań dziedziczonych nie można usunąć. Zauważ, że na obrazie poniżej przycisk Usuń jest wyszarzony. Zamiast tego spójrz na informację w obszarze **Przypisane w**. Przejdź do zasobu wskazanego w tym miejscu, aby usunąć przypisanie roli.

![Blok Użytkownicy — dostęp dziedziczony wyłącza przycisk usuwania (zrzut ekranu)](./media/role-based-access-control-configure/remove-access2.png)

## Inne narzędzia do zarządzania dostępem
Za pomocą poleceń Azure RBAC można przypisywać role i zarządzać dostępem w narzędziach innych niż witryna Azure Portal.  Skorzystaj z linków, aby dowiedzieć się więcej o wymaganiach wstępnych i zacząć korzystać z poleceń Azure RBAC.

* [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
* [Interfejs wiersza polecenia platformy Azure](role-based-access-control-manage-access-azure-cli.md)
* [Interfejs API REST](role-based-access-control-manage-access-rest.md)

## Następne kroki
* [Tworzenie raportu historii zmian dostępu](role-based-access-control-access-change-history-report.md)
* Zobacz [Wbudowane role RBAC](role-based-access-built-in-roles.md)
* Definiowanie własnych [niestandardowych ról dla kontroli dostępu opartej na rolach na platformie Azure](role-based-access-control-custom-roles.md)

<!--HONumber=Oct16_HO3-->


