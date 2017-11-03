Azure CLI 2.0 umożliwia tworzenie i zarządzanie zasobami Azure na macOS, Linux i Windows. W tym artykule szczegółowo niektóre z najczęściej używanych poleceń, aby utworzyć i zarządzać maszynach wirtualnych (VM).

W tym artykule wymaga wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). Można również użyć [powłoki chmury](/azure/cloud-shell/quickstart) z przeglądarki.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Podstawowe polecenia usługi Azure Resource Manager w interfejsie wiersza polecenia platformy Azure
Bardziej szczegółowe pomocy z przełącznikami określonego wiersza polecenia i opcjami, wpisując można użyć Pomocy online polecenia i opcje `az <command> <subcommand> --help`.

### <a name="create-vms"></a>Tworzenie maszyn wirtualnych
| Zadanie | Polecenia interfejsu wiersza polecenia platformy Azure |
| --- | --- |
| Tworzenie grupy zasobów | `az group create --name myResourceGroup --location eastus` |
| Tworzenie maszyny wirtualnej z systemem Linux | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| Tworzenie maszyny wirtualnej z systemem Windows | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a>Zarządzanie stanem maszyny Wirtualnej
| Zadanie | Polecenia interfejsu wiersza polecenia platformy Azure |
| --- | --- |
| Uruchamianie maszyny wirtualnej | `az vm start --resource-group myResourceGroup --name myVM` |
| Zatrzymywanie maszyny wirtualnej | `az vm stop --resource-group myResourceGroup --name myVM` |
| Cofanie przydziału maszyny wirtualnej | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| Ponowne uruchamianie maszyny wirtualnej | `az vm restart --resource-group myResourceGroup --name myVM` |
| Ponowne wdrażanie maszyny wirtualnej | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| Usuwanie maszyny wirtualnej | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a>Pobierz informacje dotyczące maszyny Wirtualnej
| Zadanie | Polecenia interfejsu wiersza polecenia platformy Azure |
| --- | --- |
| Wyświetlanie listy maszyn wirtualnych | `az vm list` |
| Uzyskiwanie informacji o maszynie wirtualnej | `az vm show --resource-group myResourceGroup --name myVM` |
| Używanie zasobów maszyny wirtualnej | `az vm list-usage --location eastus` |
| Pobieranie wszystkich rozmiarów maszyn wirtualnych | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a>Dyski i obrazów
| Zadanie | Polecenia interfejsu wiersza polecenia platformy Azure |
| --- | --- |
| Dodawanie dysku danych do maszyny wirtualnej | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new ` |
| Usuwanie dysku danych z maszyny wirtualnej | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| Zmienianie rozmiaru dysku | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| Tworzenie migawki dysku | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| Tworzenie obrazu maszyny wirtualnej | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| Tworzenie maszyny Wirtualnej z obrazu | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Następne kroki
Aby uzyskać dodatkowe przykłady poleceń interfejsu wiersza polecenia, zobacz [tworzenie i zarządzanie maszyn wirtualnych systemu Linux z wiersza polecenia platformy Azure](../articles/virtual-machines/linux/tutorial-manage-vm.md) samouczka.

