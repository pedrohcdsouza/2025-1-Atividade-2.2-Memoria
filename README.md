# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: Pedro Henrique Cardoso de Souza

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

## Resposta
### 1. Alocação Inicial com Best-Fit
**RAM total:** 64 KB  
**Processos:** P1 = 20 KB, P2 = 15 KB, P3 = 25 KB, P4 = 10 KB, P5 = 18 KB  

**Passo a passo do Best-Fit:**
1. **P1 (20 KB)** → Menor espaço livre que comporte 20 KB → `[0 - 20]`
2. **P2 (15 KB)** → Menor espaço livre que comporte 15 KB → `[20 - 35]`
3. **P3 (25 KB)** → Menor espaço livre que comporte 25 KB → `[35 - 60]`
4. **P4 (10 KB)** → Não cabe no espaço restante (64 - 60 = 4 KB) → vai para **memória virtual**
5. **P5 (18 KB)** → Também não cabe → vai para **memória virtual**

**Situação da RAM (inicial):**
[0 - 20] -> P1 (20 KB)
[20 - 35] -> P2 (15 KB)
[35 - 60] -> P3 (25 KB)
[60 - 64] -> Livre (4 KB)

### 2. Simular Memória Virtual (Paginação)
**Tabela de Páginas (tamanho de página = 4 KB, para simulação):**

| Processo | Página | Localização |
|----------|--------|-------------|
| P1       | 0-4    | RAM         |
| P1       | 4-8    | RAM         |
| P1       | 8-12   | RAM         |
| P1       | 12-16  | RAM         |
| P1       | 16-20  | RAM         |
| P2       | 0-4    | RAM         |
| P2       | 4-8    | RAM         |
| P2       | 8-12   | RAM         |
| P2       | 12-15  | RAM         |
| P3       | 0-4    | RAM         |
| P3       | 4-8    | RAM         |
| P3       | 8-12   | RAM         |
| P3       | 12-16  | RAM         |
| P3       | 16-20  | RAM         |
| P3       | 20-25  | RAM         |
| P4       | 0-10   | Disco       |
| P5       | 0-18   | Disco       |

### 3. Desfragmentação da RAM
Após desfragmentar (compactar os blocos para o início da RAM), a situação fica:

[0 - 20] -> P1 (20 KB)
[20 - 35] -> P2 (15 KB)
[35 - 60] -> P3 (25 KB)
[60 - 64] -> Livre (4 KB)

### 4. Questões para Reflexão

1. **Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**  
   Sim, o best-fit aproveitou melhor os blocos de memória disponíveis, colocando cada processo no menor espaço livre possível que o comportava. Como a memória estava contígua e vazia no início, o resultado foi próximo ao first-fit, mas o best-fit tende a gerar menos fragmentação externa ao longo do tempo.

2. **Como a memória virtual evitou um deadlock?**  
   Sem memória virtual, processos que não coubessem na RAM ficariam esperando indefinidamente, bloqueando a execução de outros. A memória virtual permitiu que processos excedentes fossem armazenados no disco, garantindo que todos pudessem ser carregados ou executados por paginação, evitando deadlock.

3. **Qual o impacto da desfragmentação no desempenho do sistema?**  
   A desfragmentação melhora o aproveitamento da RAM, liberando blocos contíguos maiores para alocação. No entanto, ela consome tempo de CPU e pode causar pausas temporárias, já que é preciso mover dados na memória. Em sistemas críticos, isso pode afetar a performance momentaneamente.
