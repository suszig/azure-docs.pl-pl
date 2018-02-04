## <a name="supported-distributions-and-drivers"></a>Obsługiwane dystrybucje i sterowniki


### <a name="nc-ncv2-and-nd-instances---nvidia-cuda-drivers"></a>Wystąpienia NC, NCv2 i ND - NVIDIA CUDA sterowniki
| Dystrybucja | Sterownik |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 lub 7.4<br/><br/> CentOS 7.3 lub 7.4 | NVIDIA CUDA 9.1, sterownika gałęzi R390 |

> [!IMPORTANT]
> Upewnij się, czy instalacji lub uaktualnienia do najnowszej wersji sterowników CUDA dla dystrybucji. Sterowniki starsza niż wersja R390 mogą wystąpić problemy z zaktualizowane jądra systemu Linux.
>

### <a name="nv-instances---nvidia-grid-drivers"></a>Wystąpienia NV — sterowniki NVIDIA GRID


| Dystrybucja | Sterownik |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>Oparty na systemie CentOS 7.3 | 5.2 siatki NVIDIA, sterownika gałęzi R384|

> [!NOTE]
> Microsoft ponownie dystrybuuje instalatorów sterownika NVIDIA siatki dla maszyn wirtualnych z wirtualizacją sieci. Zainstaluj tylko te sterowniki siatki na maszynach wirtualnych z wirtualizacją sieci platformy Azure. Te sterowniki obejmują licencjonowania siatki wirtualnej oprogramowania procesora GPU na platformie Azure.
>

> [!WARNING] 
> Instalacja oprogramowania innych firm na produktach systemu Red Hat może wpłynąć na warunki wsparcia systemu Red Hat. Zobacz [artykuł bazy wiedzy systemu Red Hat](https://access.redhat.com/articles/1067).
>
