## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

Utwórz maszynę wirtualną za pomocą polecenia [az vm create](/cli/azure/vm#create). 

Następujący przykład umożliwia utworzenie maszyny wirtualnej o nazwie *myVM* i kluczy SSH, jeśli jeszcze nie istnieją w domyślnej lokalizacji kluczy. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Po utworzeniu maszyny wirtualnej w interfejsie wiersza polecenia platformy Azure zostanie wyświetlona informacja podobna do następującej. Zwróć uwagę na element `publicIpAddress`. Ten adres jest używany na potrzeby uzyskiwania dostępu do maszyny wirtualnej.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a>Otwieranie portu 80 na potrzeby ruchu w sieci Web 

Domyślnie tylko połączeń SSH mają dostęp do maszyn wirtualnych systemu Linux wdrożonych na platformie Azure. Ponieważ ta maszyna wirtualna ma być serwerem sieci web, należy otworzyć port 80 z Internetu. Otwórz odpowiedni port za pomocą polecenia [az vm open-port](/cli/azure/vm#open-port).  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>Łączenie z maszyną wirtualną za pośrednictwem protokołu SSH


Jeśli nie znasz już publicznego adresu IP maszyny Wirtualnej, należy uruchomić [az sieci ip publicznego listy](/cli/azure/network/public-ip#list) polecenia:


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Użyj następującego polecenia, aby utworzyć sesję SSH z maszyną wirtualną. Zastąp poprawne publiczny adres IP maszyny wirtualnej. W tym przykładzie adres IP jest *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

