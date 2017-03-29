



Aby umożliwić porównywanie wydajności obliczeniowej (procesora CPU) jednostek SKU platformy Azure, stworzyliśmy koncepcję jednostki obliczeniowej platformy Azure (Azure Compute Unit, ACU). Ten parametr pomoże łatwo zidentyfikować jednostkę SKU, która najprawdopodobniej spełni określone potrzeby związane z wydajnością.  Jednostka ACU jest obecnie standaryzowana na małej maszynie wirtualnej (Standardowa_A1) jako równa 100, a wszystkie pozostałe jednostki SKU reprezentują w przybliżeniu, o ile szybciej dana jednostka SKU może uruchomić standardowy test porównawczy. 

> [!IMPORTANT]
> Wartość ACU jest tylko wskazówką.  Wyniki dla konkretnego obciążenia mogą się różnić. 
> 
> 

<br>

| Rodzina SKU | ACU/rdzeń |
| --- | --- |
| [A0](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |50 |
| [A1–A4](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |100 |
| [A5–A7](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |100 |
| [A1_v2–A8_v2](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |100 |
| [A2m_v2–A8m_v2](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |100 |
| [A8–A11](../articles/virtual-machines/virtual-machines-windows-sizes-hpc.md) |225* |
| [D1–D14](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160 |
| [D1_v2–D15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |210 - 250* |
| [DS1–DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 |
| [DS1_v2–DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* |
| [F1–F16](../articles/virtual-machines/virtual-machines-windows-sizes-compute.md) |210-250* |
| [F1s–F16s](../articles/virtual-machines/virtual-machines-windows-sizes-compute.md) |210-250* |
| [G1–G5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* |
| [GS1–GS5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* |
| [H](../articles/virtual-machines/virtual-machines-windows-sizes-hpc.md) |290 - 300* |
| [L4s–L32s](../articles/virtual-machines/virtual-machines-windows-sizes-storage.md) |180 - 240* |

Jednostki ACU oznaczone gwiazdką (*) wykorzystują technologię Intel® Turbo w celu zwiększenia częstotliwości zegara procesora CPU i zapewniania większej wydajności.  Skala zwiększenia wydajności może się różnić w zależności od rozmiaru maszyny wirtualnej, obciążenia i innych obciążeń uruchomionych na tym samym hoście.
