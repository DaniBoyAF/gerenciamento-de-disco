# Relatório do laboratório — Gerenciamento de Disco: HDD, SSD SATA e NVMe

**Sala nº:** ___

| | Integrante 1 | Integrante 2 |
| --- | --- | --- |
| Nome | Daniel Andrade Ferreira | ___ |
| Sistema operacional | Windows via WSL | ___ |
| Tipo de disco (HDD, SSD SATA ou NVMe) | NVMe | ___ |
| Modelo do disco | KIOXIA KBG40ZNS256G | ___ |
| Ferramenta usada (fio, CrystalDiskMark, WSL) | fio no WSL | ___ |

## Passo 1 — Os dois testes

| Teste | Integrante | IOPS | Throughput (MB/s) | Latência média (com unidade) |
| --- | --- | --- | --- | --- |
| A — Sequencial 1 MB | 1 | 128 | 135 MB/s | 7,75 ms |
| B — Aleatório 4 KB | 1 | 2600 | 10,6 MB/s | 12,301 ms |
| A — Sequencial 1 MB | 2 | | | |
| B — Aleatório 4 KB | 2 | | | |

**Pergunta 1.** Quantas vezes o throughput do teste A é maior que o do B? Quantas vezes o IOPS do B é maior que o do A? Qual era a resposta certa da enquete?

> O throughput de A é aproximadamente 12,74 vezes maior que o de B.
> O IOPS de B é aproximadamente 20,31 vezes maior que o de A.
> A resposta correta foi que o throughput sequencial é bem maior, enquanto o aleatório oferece muito mais IOPS.

**Conferência.** Escolham um teste e verifiquem: throughput ≈ IOPS × tamanho do bloco.

> 2600 IOPS × 0,004 MB = 10,4 MB/s (medido: 10,6 MB/s)

## Passo 2 — O efeito das filas

| Integrante | IOPS com `iodepth=1` | IOPS com `iodepth=32` | Quantas vezes maior |
| --- | --- | --- | --- |
| 1 | 2514 | 3183 | 1,27 |
| 2 | | | |

**Pergunta 2.** O IOPS mudou muito? Por quê?

> O IOPS aumentou aproximadamente 27%, não uma mudança muito grande.
> O iodepth maior mantém mais pedidos em fila e permite melhor paralelismo no NVMe,
> mas o WSL, a máquina virtual e o armazenamento compartilhado podem limitar o ganho.

## Passo 3 — A armadilha do cache

| Integrante | Throughput com `--direct=1` (do passo 1) | Throughput sem `--direct=1` (2ª rodada) |
| --- | --- | --- |
| 1 | | |
| 2 | | |

**Pergunta 3.** O número explodiu? O que vocês mediram de verdade?

> O número não explodiu: as medições ficaram próximas, com 135 MB/s e 139 MB/s.
> Sem --direct=1, o cache do sistema operacional pode atender parte das leituras,
> fazendo a medição misturar cache e armazenamento físico.

## Passo 4 — Desafio

| Aplicação | Métrica que decide | Disco escolhido | Número que justifica |
| --- | --- | --- | --- |
| A) Servidor de vídeo 4K | Throughput (MB/s) | NVMe | 135 MB/s |
| B) Banco de transações | IOPS e latência | NVMe | 2600 IOPS; 12,301 ms |

**Pergunta 4.** Justifiquem as duas escolhas em até quatro linhas, usando os números medidos.

> Para vídeo 4K, escolhemos NVMe porque medimos 135 MB/s em leitura sequencial.
> Para o banco, escolhemos NVMe porque medimos 2600 IOPS em leitura aleatória
> de 4 KB, com latência média de 12,301 ms.

## Para o debate

A frase que a dupla vai dizer na sala principal (1 minuto):

>
