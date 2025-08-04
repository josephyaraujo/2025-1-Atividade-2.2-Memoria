# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: [Josephy C. Araujo](https://github.com/josephyaraujo)

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

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
