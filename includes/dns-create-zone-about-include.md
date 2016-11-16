Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Aby uruchomić hosting domeny, trzeba utworzyć strefę DNS. Każdy rekord DNS utworzony dla określonej domeny będzie znajdować się w strefie DNS danej domeny.

Na przykład domena „contoso.com” może zawierać wiele rekordów DNS, takich jak „mail.contoso.com” (dla serwera poczty) i „www.contoso.com” (dla witryny sieci Web).

## <a name="a-namenamesaabout-dns-zone-names"></a><a name="names"></a>Informacje o nazwach strefy DNS
* Nazwa strefy musi być unikatowa w obrębie grupy zasobów, a strefa nie może wcześniej istnieć. W przeciwnym razie operacja zakończy się niepowodzeniem.
* Daną nazwę strefy można użyć ponownie w innej grupie zasobów lub w ramach innej subskrypcji platformy Azure.
* Jeśli wiele stref ma taką samą nazwę, do każdego wystąpienia zostaną przypisane adresy serwera o innej nazwie i tylko jedno wystąpienie może być delegowane z domeny nadrzędnej. Więcej informacji można znaleźć w temacie [Delegowanie domeny do usługi DNS platformy Azure](../articles/dns/dns-domain-delegation.md).


<!--HONumber=Nov16_HO2-->


