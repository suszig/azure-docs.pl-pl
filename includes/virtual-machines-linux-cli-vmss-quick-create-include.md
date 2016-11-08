Osoby, które nie skorzystały jeszcze z [bezpłatnej wersji próbnej subskrypcji Azure](https://azure.microsoft.com/pricing/free-trial/) i [interfejsu wiersza polecenia Azure](../articles/xplat-cli-install.md) [powiązanych z kontem usługi Azure](../articles/xplat-cli-connect.md), mogą je zarejestrować. Po wykonaniu tej czynności można uruchomić następujące polecenia w celu szybkiego utworzenia zestawu skali:

```bash
# make sure we are in Resource Manager mode (https://azure.microsoft.com/en-us/documentation/articles/resource-manager-deployment-model/)
azure config mode arm

# quick-create a scale set
#
# generic syntax:
# azure vmss quick-create -n SCALE-SET-NAME -g RESOURCE-GROUP-NAME -l LOCATION -u USERNAME -p PASSWORD -C INSTANCE-COUNT -Q IMAGE-URN
#
# example:
azure vmss quick-create -n negatvmss -g negatvmssrg -l westus -u negat -p P4ssw0rd -C 5 -Q Canonical:UbuntuServer:14.04.4-LTS:latest
```

Aby dostosować lokalizację lub wartość image-urn, należy zapoznać się z poleceniami `azure location list` i `azure vm image {list-publishers|list-offers|list-skus|list|show}`.

Gdy polecenie zostanie zwrócone, nastąpi utworzenie zestawu skali. Ten zestaw skali będzie miał moduł równoważenia obciążenia z regułami translatora adresów sieciowych mapującymi port 50 000+i modułu równoważenia obciążenia na port 22 maszyny wirtualnej i. W związku z tym po ustaleniu w pełni kwalifikowanej nazwy domeny (FQDN) modułu równoważenia obciążenia będzie możliwe nawiązanie połączenia z maszynami wirtualnymi za pośrednictwem protokołu SSH:

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

# now that we have the name of the load balancer, we can show the details to find which Public IP (PIP) is associated to it
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

<!--HONumber=Oct16_HO3-->


