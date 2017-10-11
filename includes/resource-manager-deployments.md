## <a name="incremental-and-complete-deployments"></a>Przyrostowe i pełne wdrożenia
Podczas wdrażania zasobów, należy określić, że wdrożenie jest aktualizacji przyrostowej i pełnej aktualizacji. Główną różnicą między tych dwóch trybów jest jak Resource Manager obsługuje istniejących zasobów w grupie zasobów, które nie znajdują się w szablonie:

* W trybie pełnej, Menedżer zasobów **usuwa** zasobów, które istnieją w grupie zasobów, ale nie są określone w szablonie. 
* W trybie przyrostowych, Menedżer zasobów **pozostawia niezmienione** zasobów, które istnieją w grupie zasobów, ale nie są określone w szablonie.

Dla obu trybów Resource Manager podejmie próbę zapewnić wszystkich zasobów określone w szablonie. Jeśli zasób już istnieje w grupie zasobów i jego ustawienia są takie same jak, wykonanie tej operacji skutkuje bez zmian. Jeśli zmienisz ustawienia zasobu zasobu jest udostępniane z tych nowych ustawień. Jeśli próbujesz zaktualizować lokalizację i typ istniejącego zasobu, wdrożenie zakończy się niepowodzeniem z powodu błędu. Zamiast tego należy wdrożyć nowy zasób z lokalizacją lub wpisz, że należy.

Domyślnie usługi Resource Manager korzysta z trybu przyrostowe.

Aby zilustrować różnica między trybami przyrostowe i pełne, rozważmy następujący scenariusz.

**Istniejąca grupa zasobów** zawiera:

* Zasobów A
* Zasób B
* Zasób C

**Szablon** definiuje:

* Zasobów A
* Zasób B
* Zasób D

Po wdrożeniu w **przyrostowe** trybie grupa zasobów zawiera:

* Zasobów A
* Zasób B
* Zasób C
* Zasób D

Po wdrożeniu w **pełną** trybie C zasób zostanie usunięty. Grupa zasobów zawiera:

* Zasobów A
* Zasób B
* Zasób D
