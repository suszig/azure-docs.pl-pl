## <a name="a-namex-subapeering-across-subscriptions"></a><a name="x-sub"></a>Komunikacja równorzędna między subskrypcjami
W tym scenariuszu utworzysz komunikację równorzędną między dwiema sieciami wirtualnymi istniejącymi w różnych subskrypcjach.

![scenariusz komunikacji między subskrypcjami](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Komunikacja równorzędna sieci wirtualnych bazuje na kontroli dostępu do autoryzacji opartej na roli (RBAC, role-based access control). Na potrzeby scenariusza komunikacji między subskrypcjami należy najpierw udzielić wystarczających uprawnień użytkownikom, którzy będą tworzyć połączenie komunikacji równorzędnej.

> [!NOTE]
> Jeśli ten sam użytkownik ma uprawnienia do korzystania z obu subskrypcji, można pominąć opisane poniżej kroki od 1 do 4.
> 
> 



<!--HONumber=Feb17_HO1-->


