## <a name="prerequisites"></a>Wymagania wstępne

Osoby, które nie skorzystały jeszcze z [bezpłatnej wersji próbnej subskrypcji Azure](https://azure.microsoft.com/pricing/free-trial/) i [interfejsu wiersza polecenia Azure 1.0](../articles/cli-install-nodejs.md) [powiązanych z kontem usługi Azure](../articles/xplat-cli-connect.md), mogą je zarejestrować. Upewnij się, że interfejs wiersza polecenia platformy Azure działa w trybie usługi Resource Manager, w następujący sposób:

```azurecli
azure config mode arm
```

## <a name="create-the-scale-set"></a>Tworzenie zestawu skalowania

Teraz utwórz zestaw skalowania za pomocą polecenia `azure vmss quick-create`. Poniższy przykład obejmuje tworzenie zestawu skalowania systemu Linux o nazwie `myVMSS` z pięcioma wystąpieniami maszyny wirtualnej w grupie zasobów o nazwie `myResourceGroup`:

```azurecli
azure vmss quick-create -n myVMSS -g myResourceGroup -l westus \
    -u ops -p P@ssw0rd! \
    -C 5 -Q Canonical:UbuntuServer:16.04-LTS:latest
```

Poniższy przykład tworzy zestaw skalowania systemu Windows z taką samą konfiguracją:

```azurecli
azure vmss quick-create -n myVMSS -g myResourceGroup -l westus \
    -u ops -p P@ssw0rd! \
    -C 5 -Q MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest
```

Aby dostosować lokalizację lub wartość image-urn, należy zapoznać się z poleceniami `azure location list` i `azure vm image {list-publishers|list-offers|list-skus|list|show}`.

Polecenie to zwraca wynik po zakończeniu wdrażania zestawu skalowania. Zestaw skalowania ma teraz moduł równoważenia obciążenia z regułami translatora adresów sieciowych mapującymi port 50 000+i modułu równoważenia obciążenia na port 22 maszyny wirtualnej i. W związku z tym po ustaleniu w pełni kwalifikowanej nazwy domeny (FQDN) modułu równoważenia obciążenia będzie możliwe nawiązanie połączenia z maszynami wirtualnymi za pośrednictwem protokołu SSH:

```bash
# (if you decide to run this as a script, please invoke using bash)

# list load balancers in the resource group we created
#
# generic syntax:
# azure network lb list -g RESOURCE-GROUP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb list -g negatvmssrg | grep negatvmssrg)
split_line=( $line )
lb_name=${split_line[1]}

# now that we have the name of the load balancer, we can show the details to find which Public IP (PIP) is 
# associated to it
#
# generic syntax:
# azure network lb show -g RESOURCE-GROUP-NAME -n LOAD-BALANCER-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb show -g negatvmssrg -n $lb_name | grep loadBalancerFrontEnd)
split_line=( $line )
pip_name=${split_line[4]}

# now that we have the name of the public IP address, we can show the details to find the FQDN
#
# generic syntax:
# azure network public-ip show -g RESOURCE-GROUP-NAME -n PIP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network public-ip show -g negatvmssrg -n $pip_name | grep FQDN)
split_line=( $line )
FQDN=${split_line[3]}

# now that we have the FQDN, we can use ssh on port 50,000+i to connect to VM i (where i is 0-indexed)
#
# example to connct via ssh into VM "0":
ssh -p 50000 negat@$FQDN
```