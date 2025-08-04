### S.O. 2025.1 - Atividade 2.02
# **Gestão de Memória**
### Aluno: Josephy Cruz Araújo 
### Data: 04/08/2025

## Resposta:
Foram analisados 5 processos cuja soma total (88 KB) excede a capacidade física da RAM, exigindo estratégias de alocação e substituição eficientes.

### **1. Alocação Inicial com Best-Fit**  
**Processos:**  
| Processo | Tamanho (KB)|  
|----------|-------------|  
| P1       | 20          |  
| P2       | 15          |  
| P3       | 25          |  
| P4       | 10          |  
| P5       | 18          |  

**Alocação na RAM (64 KB):**  
1. **P1 (20 KB)**: Alocado em `[0KB - 20KB]`  
2. **P2 (15 KB)**: Alocado em `[20KB - 35KB]`     
3. **P4 (10 KB)**: Alocado em `[35KB - 45KB]`  
4. **P5 (18 KB)**: Alocado em `[45KB - 63KB]`
5. **P3 (25 KB)**: Não caberá na memória, processo não será alocado → **Enviado para disco (memória virtual)**

**Estado Final da RAM:**  
[0-20]: P1 | [20-35]: P2 | [35-45]: P4 | [45-63]: P5 | [63-64]: Livre (1 KB)

**Processo Paginado:** P3 (25 KB) no disco.  

---

### **2. Simulação de Memória Virtual (Paginação)**  
Como a RAM não comporta todos os processos, utiliza-se a **memória virtual** via paginação. Pra esse exemplo, não foi considerado tamanho fixo por página.

**Tabela de Páginas:**  
| Processo | Tamanho (KB) | Páginas na RAM | Páginas no Disco |
|----------|--------------|----------------|------------------|
| P1       | 20           | 1              | 0                |
| P2       | 15           | 1              | 0                |
| P4       | 10           | 1              | 0                |
| P5       | 18           | 1              | 0                |
| P3       | 25           | 0              | 1                |

**Mecanismo:**  
- Quando P3 for requisitado, ocorrerá um **page fault**, e o SO substituirá uma página na RAM para trazer P3 do disco.  

---

### **3. Desfragmentação da RAM**  
**Antes:** A memória está ocupada de forma contígua. Na RAM há 1 KB livre ao final, insuficiente para alocar P3. Nenhum processo novo pode ser alocado.  

**Após desfragmentação:** Todos os processos são compactados no início, mas o espaço livre continua sendo de apenas 1 KB.

**Total utilizado:**  
- P1 + P2 + P4 + P5 = **63 KB**  
- **Livre:** 1 KB

**Resultado:** Mesmo com a desfragmentação, ainda não é possível alocar P3 na RAM. O processo continuará sendo mantido em memória virtual.

**Conclusão:** 
A desfragmentação só reorganiza a RAM existente, não cria espaço adicional.
Para alocar P3, seria necessário:
- Swap out de outro processo (ex.: P4 ou P5) ou
- Aumentar artificialmente a RAM (o que não reflete a realidade).

---

## **Questões para Reflexão**  

### **1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**  
No cenário utilizado como exemplo o best-fit permitiu um uso mais eficiente da RAM, pois alocou os processos nos menores espaços livres possíveis, reduzindo a fragmentação interna. No entanto, como os processos já foram inseridos sequencialmente e a RAM estava inicialmente vazia, a diferença entre best-fit e first-fit foi mínima. O worst-fit provavelmente deixaria grandes espaços não aproveitados no início.
- **Best-Fit** minimizou fragmentação inicial, mas deixou espaços inúteis (1 KB).  
- **First-Fit** teria alocado P3 em `[35-64]` (29 KB), desperdiçando 4 KB.  
- **Worst-Fit** seria pior neste cenário.  

### **2. Como a memória virtual evitou um deadlock?**  
Evitou **deadlock** ao permitir execução de P3 via paginação. A memória virtual permitiu que processos que não cabem na RAM fossem armazenados no disco. Isso evita que o sistema fique bloqueado esperando a liberação de espaço físico, o que poderia levar a um deadlock. Com paginação, os processos podem ser parcialmente ou totalmente executados em disco, garantindo a continuidade do sistema.   

### **3. Qual o impacto da desfragmentação no desempenho do sistema?**  
A desfragmentação melhora a eficiência da RAM ao agrupar os blocos utilizados, reduzindo fragmentação externa. No entanto, ela não liberou espaço suficiente para alocar novos processos neste caso. Além disso, desfragmentar exige movimentar processos na memória, o que consome tempo e recursos do sistema, podendo degradar temporariamente o desempenho.
- **Vantagem:** Espaço contíguo liberado.  
- **Custo:** Overhead de CPU para mover processos.  
