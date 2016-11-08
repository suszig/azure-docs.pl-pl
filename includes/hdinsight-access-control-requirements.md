Jeśli korzystasz z subskrypcji platformy Azure, której nie jesteś administratorem/właścicielem, na przykład subskrypcji należącej do firmy, przed rozpoczęciem wykonywania czynności opisanych w tym dokumencie musisz sprawdzić następujące elementy:

* Identyfikator logowania platformy Azure musi mieć dostęp co najmniej o roli **Współautor** do grupy zasobów platformy Azure, która jest używana podczas tworzenia usługi HDInsight (oraz innych zasobów platformy Azure).
* Dostawca dla używanego zasobu musi zostać wcześniej zarejestrowany przez osobę z dostępem do subskrypcji Azure co najmniej o roli **Współautor**. Rejestracja zasobu ma miejsce, gdy użytkownik o roli Współautor z dostępem do subskrypcji tworzy zasób po raz pierwszy w subskrypcji. Można to osiągnąć również bez tworzenia zasobu, [rejestrując dostawcę za pomocą interfejsu REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Więcej informacji o zarządzaniu dostępem można znaleźć w następujących dokumentach:

* [Wprowadzenie do zarządzania dostępem w witrynie Azure Portal](../articles/active-directory/role-based-access-control-what-is.md)
* [Zarządzanie dostępem do zasobów subskrypcji platformy Azure za pomocą przypisań ról](../articles/active-directory/role-based-access-control-configure.md)

<!--HONumber=Sep16_HO5-->


