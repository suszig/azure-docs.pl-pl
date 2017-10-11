


## <a name="tagging-a-virtual-machine-through-templates"></a>Znakowanie maszynę wirtualną za pomocą szablonów
Po pierwsze Przyjrzyjmy się znakowanie za pomocą szablonów. [Ten szablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) umieszcza tagi w następujących zasobach: obliczeniowych (maszyna wirtualna), magazynu (konta magazynu) i sieci (publicznego adresu IP sieci wirtualnej i interfejsu sieciowego). Ten szablon jest przeznaczony dla maszyny Wirtualnej systemu Windows, ale mogą być dostosowywane dla maszyn wirtualnych systemu Linux.

Kliknij przycisk **wdrażanie na platformie Azure** przycisk z [łącze szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Spowoduje to przejście do [portalu Azure](https://portal.azure.com/) którym można wdrożyć tego szablonu.

![Proste wdrożenie przy użyciu tagów](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Ten szablon obejmuje następujące znaczniki: *działu*, *aplikacji*, i *utworzony przez*. Użytkownik może Dodawanie/edytowanie znaczniki bezpośrednio w szablonie Jeśli chcesz różnych nazwach.

![Azure tagów w szablonie](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Jak widać, tagi są definiowane jako pary klucz wartość oddzielone dwukropkiem (:). Tagi musi być zdefiniowana w następującym formacie:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Zapisz plik szablonu, po zakończeniu edycji za pomocą tagów wybranych przez użytkownika.

Następnie w **Edytuj parametry** sekcji można wypełnić wartości dla tagów.

![Edytowanie tagów w portalu Azure](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Kliknij przycisk **Utwórz** do wdrożenia tego szablonu o wartości tagu.

## <a name="tagging-through-the-portal"></a>Znakowanie za pośrednictwem portalu
Po utworzeniu zasobów za pomocą tagów, można wyświetlać, dodawania i usuwania tagów w portalu.

Wybierz ikonę znaczniki, aby wyświetlić tagów:

![Ikona tagów w portalu Azure](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Dodaj nowy znacznik za pośrednictwem portalu, definiując własne parę klucza i wartości i zapisz go.

![Dodaj nowy znacznik w portalu Azure](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Twoje nowy znacznik powinien zostać wyświetlony na liście tagów dla zasobu.

![Nowy znacznik zapisane w portalu Azure](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

