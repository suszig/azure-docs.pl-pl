Można użyć subskrypcji platformy Azure, dla której nie jesteś administratorem ani właścicielem, takiej jak subskrypcja należąca do firmy. W takiej sytuacji w celu wykonania kroków opisanych w tym artykule musisz sprawdzić, czy zapewniono następujące kwestie:

* Dostęp na poziomie współautora. Aby zalogować się do platformy Azure, potrzebny jest co najmniej dostęp współautora do grupy zasobów platformy Azure. Ta grupa zasobów służy do tworzenia klastra usługi HDInsight i innych zasobów platformy Azure.
* Rejestracja dostawcy. Dostawca dla używanego zasobu musi zostać wcześniej zarejestrowany przez osobę z dostępem do subskrypcji Azure co najmniej na poziomie współautora. Rejestracja zasobu ma miejsce, gdy użytkownik o roli Współautor z dostępem do subskrypcji tworzy zasób po raz pierwszy w subskrypcji. Można to osiągnąć również bez tworzenia zasobu, [rejestrując dostawcę za pośrednictwem interfejsu REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Więcej informacji o zarządzaniu dostępem można znaleźć w następujących artykułach:

* [Wprowadzenie do zarządzania dostępem w witrynie Azure Portal](../articles/active-directory/role-based-access-control-what-is.md)
* [Zarządzanie dostępem do zasobów subskrypcji platformy Azure za pomocą przypisań ról](../articles/active-directory/role-based-access-control-configure.md)
