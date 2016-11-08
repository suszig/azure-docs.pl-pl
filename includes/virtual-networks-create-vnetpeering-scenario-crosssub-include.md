## Komunikacja równorzędna między subskrypcjami
W tym scenariuszu utworzysz komunikację równorzędną między dwiema sieciami wirtualnymi należącymi do różnych subskrypcji.

![scenariusz komunikacji między subskrypcjami](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Komunikacja równorzędna sieci wirtualnych bazuje na kontroli dostępu do autoryzacji opartej na roli (RBAC). Na potrzeby scenariusza komunikacji między subskrypcjami należy najpierw udzielić wystarczających uprawnień użytkownikom, którzy będą tworzyć połączenie komunikacji równorzędnej sieci wirtualnych:

> [!NOTE]
> Jeśli ten sam użytkownik jest uprzywilejowany do korzystania z obu subskrypcji, można pominąć kroki od 1 do 4 opisane poniżej.
> 
> 

<!--HONumber=Sep16_HO3-->


