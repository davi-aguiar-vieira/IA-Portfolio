## Representação Atômica vs. Representação Fatorada

#### **Detalhamento das Representações**

1. **Representação Atômica**:
   - Cada estado é tratado como uma entidade indivisível, sem nenhuma estrutura interna.
   - Útil quando os estados podem ser enumerados diretamente e são relativamente simples.
   - Os algoritmos que lidam com estados atômicos exploram os estados como um todo, sem conhecimento sobre possíveis subcomponentes ou relações internas.

   **Exemplo de uso**:  
   Um tabuleiro de jogo onde cada configuração possível é representada como um estado único (e.g., todas as posições possíveis em um jogo da velha).

2. **Representação Fatorada**:
   - O estado é descrito por meio de variáveis e valores associados a essas variáveis.
   - Permite expressar de maneira compacta problemas com muitos estados possíveis, evitando a necessidade de enumerar todos os estados.
   - Usada em problemas mais complexos, onde as relações entre variáveis desempenham papel importante.

   **Exemplo de uso**:  
   Um Sudoku, onde cada célula é uma variável com um domínio (os números de 1 a 9) e restrições definindo as regras do jogo.

#### **Comparação**

| **Aspecto**                    | **Representação Atômica**                                    | **Representação Fatorada**                                 |
|---------------------------------|-------------------------------------------------------------|-----------------------------------------------------------|
| **Definição**                   | Estados tratados como caixas pretas, sem estrutura interna. | Estados descritos por variáveis com valores associados.   |
| **Modelagem**                   | Enumeração de estados únicos.                              | Composição de estados a partir de variáveis.              |
| **Escalabilidade**              | Pouco escalável para problemas com muitos estados.          | Mais escalável, já que explora relações entre variáveis.  |
| **Flexibilidade**               | Rígida; os estados precisam ser definidos explicitamente.    | Flexível; permite mudanças no modelo alterando variáveis. |
| **Eficiência**                  | Ineficiente para grandes espaços de estados.                | Mais eficiente para problemas com muitas restrições.      |
| **Aplicabilidade**              | Problemas simples ou de baixa complexidade.                 | Problemas complexos com múltiplas restrições.             |
| **Exemplo de Problema**         | Labirinto, Jogo da Velha.                                   | Sudoku, Agendamento, Quadrados Mágicos.                  |
| **Tratamento das Restrições**   | Restrições implícitas entre estados.                        | Restrições explícitas entre variáveis.                    |
| **Espaço de Estados**           | Enumerado explicitamente.                                   | Representado de forma compacta.                           |
| **Complexidade de Representação** | Simples.                                                  | Mais detalhada e estruturada.                             |

---

A escolha entre as duas representações depende do problema. Se o espaço de estados puder ser gerenciado e as relações entre componentes forem simples, a **representação atômica** é suficiente. Por outro lado, problemas com estados complexos e interdependências são mais bem representados por uma **abordagem fatorada**.

**Representação Atômica vs. Fatorada em CSPs**

Na representação atômica, os estados são tratados como entidades indivisíveis e sem estrutura interna, ou seja, são considerados "caixas pretas". Cada estado é único e não possui variáveis específicas que o definam. Esse tipo de representação é útil para problemas onde os estados são simples ou podem ser enumerados.

Por outro lado, a representação fatorada considera os estados como combinações de valores de variáveis. Cada estado é definido por um conjunto de variáveis que possuem valores específicos. Essa abordagem é mais eficiente e flexível, pois permite modelar problemas complexos com múltiplas restrições, explorando as relações entre as variáveis.

Os problemas que utilizam representações fatoradas geralmente caem na categoria de **problemas de satisfação de restrições** (Constraint Satisfaction Problems - CSP). Em CSPs, temos:
- **Variáveis**: Conjuntos de elementos que queremos atribuir valores.
- **Domínio**: Os possíveis valores para cada variável.
- **Restrições**: Condições que definem quais combinações de valores são permitidas.

Resolver um CSP significa encontrar uma atribuição de valores para todas as variáveis que satisfaça todas as restrições.

---

**Algortimo de CSP: Quebra-cabeça de Quadrado Mágico**

Algoritmo para formar um quadrado mágico \(3 \times 3\). Um quadrado mágico é uma matriz onde a soma dos números em cada linha, coluna e diagonal principal é a mesma.

O código  utiliza o módulo `constraint` do Python para resolver o problema:

```python
from constraint import Problem

def magic_square_csp(size):
    # Soma mágica para um quadrado mágico de tamanho N
    magic_sum = size * (size**2 + 1) // 2
    
    # Criar o problema CSP
    problem = Problem()
    
    # Adicionar variáveis (uma para cada célula)
    variables = [(i, j) for i in range(size) for j in range(size)]
    domain = list(range(1, size**2 + 1))
    problem.addVariables(variables, domain)
    
    # Restrições de soma para linhas
    for i in range(size):
        problem.addConstraint(lambda *row: sum(row) == magic_sum, [(i, j) for j in range(size)])
    
    # Restrições de soma para colunas
    for j in range(size):
        problem.addConstraint(lambda *col: sum(col) == magic_sum, [(i, j) for i in range(size)])
    
    # Restrições de soma para as diagonais
    problem.addConstraint(lambda *diag1: sum(diag1) == magic_sum, [(i, i) for i in range(size)])
    problem.addConstraint(lambda *diag2: sum(diag2) == magic_sum, [(i, size - i - 1) for i in range(size)])
    
    # Restrições de valores únicos
    problem.addConstraint(lambda *vals: len(set(vals)) == len(vals), variables)
    
    # Resolver o problema
    solutions = problem.getSolutions()
    return solutions

# Executar para um quadrado mágico 3x3
solutions = magic_square_csp(3)

# Exibir uma solução (se existir)
if solutions:
    for var, value in sorted(solutions[0].items()):
        print(f"Posição {var}: {value}")
    print("\nQuadrado mágico:")
    for i in range(3):
        print([solutions[0][(i, j)] for j in range(3)])
else:
    print("Nenhuma solução encontrada.")
```

**Passos**:
1. **Definição das Variáveis**: Cada célula do quadrado mágico é tratada como uma variável.
2. **Domínio**: Os valores possíveis para as variáveis são números de 1 a \(N^2\).
3. **Restrições**:
   - A soma de cada linha, coluna e diagonal deve ser igual à soma mágica.
   - Os valores em todas as células devem ser únicos.
4. **Solução**: O módulo `constraint` resolve o problema e retorna as soluções possíveis.

---

## **Definindo Problemas de Satisfação de Condições (CSP)**

Os problemas de satisfação de condições (CSP - Constraint Satisfaction Problems) são uma classe poderosa e amplamente utilizada em Inteligência Artificial (IA) para resolver problemas que podem ser representados como combinações de variáveis sujeitas a restrições. CSPs fornecem uma abordagem estruturada para representar e resolver problemas complexos, com grande aplicabilidade em domínios como agendamento, planejamento, quebra-cabeças e alocação de recursos.


### **Componentes de um CSP**

Um CSP é definido formalmente por três componentes principais:

1. **Variáveis (x)**:
   - Um conjunto x = {x1, x2, ..., xn} de variáveis.
   - Cada variável representa um elemento do problema que requer uma atribuição de valor.

   **Exemplo**: No problema do Sudoku, cada célula da grade é uma variável.

2. **Domínios (d)**:
   - Um conjunto d = {d1, d2, ..., dn}, onde cada di é o domínio associado à variável xi.
   - O domínio é o conjunto de valores possíveis que podem ser atribuídos a uma variável.

   **Exemplo**: Em um Sudoku, o domínio de cada variável (célula) é o conjunto {1, 2, ..., 9}.

3. **Condições (c)**:
   - Um conjunto de restrições c = {c1, c2, ..., ck}, onde cada restrição especifica combinações permitidas de valores para um subconjunto de variáveis.
   - As condições podem ser unárias (envolvendo uma única variável) ou mais complexas (envolvendo várias variáveis).

   **Exemplo**: No Sudoku, uma restrição garante que números em uma linha, coluna ou bloco 3x3 sejam únicos.


### **Funcionamento e Solução**

A resolução de CSPs envolve a busca por uma atribuição de valores para as variáveis que satisfaça todas as restrições do problema. Os algoritmos de busca em CSP se destacam porque:
- Aproveitam a estrutura de estados para simplificar a busca.
- Utilizam heurísticas e técnicas específicas de domínio, poda de espaço de busca para eliminar combinações inválidas de forma eficiente.

### **Por que CSPs são Poderosos?**

1. **Estruturação do Problema**:
   - A divisão em variáveis, domínios e restrições permite modelar problemas de maneira clara e intuitiva.
   
2. **Redução do Espaço de Busca**:
   - CSPs eliminam grandes porções do espaço de busca, analisando restrições que descartam combinações inviáveis logo no início do processo.

3. **Flexibilidade**:
   - Muitos problemas podem ser representados como CSPs, e o modelo de transição (como os estados mudam) pode ser deduzido diretamente a partir da descrição do problema.

4. **Aplicação de Heurísticas**:
   - Heurísticas, como "variável mais restritiva" (MRV) ou "ordem mínima de domínio", podem ser aplicadas para acelerar a busca.


### **Algoritmo de CSP**

O exemplo a seguir resolve o problema das N Rainhas, onde devemos posicionar N rainhas em um tabuleiro N x N, de forma que nenhuma rainha ataque outra. Este é um problema clássico de CSP.

#### Código:

```python
from constraint import Problem

def n_queens_csp(n):
    # Criar o problema
    problem = Problem()
    
    # Adicionar variáveis (uma para cada rainha, representando a linha)
    # Cada variável pode estar em uma coluna diferente (domínio: 0 a n-1)
    problem.addVariables(range(n), range(n))
    
    # Adicionar restrições:
    # 1. Nenhuma rainha pode estar na mesma coluna
    problem.addConstraint(lambda *cols: len(set(cols)) == n, range(n))
    
    # 2. Nenhuma rainha pode atacar na diagonal
    def no_diagonal_attack(var1, var2, col1, col2):
        return abs(var1 - var2) != abs(col1 - col2)
    
    for i in range(n):
        for j in range(i + 1, n):
            problem.addConstraint(no_diagonal_attack, (i, j))
    
    # Resolver o problema
    solutions = problem.getSolutions()
    return solutions

# Executar para 8 rainhas
solutions = n_queens_csp(8)

# Exibir uma solução (se existir)
if solutions:
    print("Solução encontrada:")
    for var, col in sorted(solutions[0].items()):
        print(f"Rainha {var + 1} na coluna {col + 1}")
else:
    print("Nenhuma solução encontrada.")
```
---

## **Tipos de Condições em CSPs**

Os **Problemas de Satisfação de Condições (CSP)** são construídos com base em restrições que regem quais combinações de valores para variáveis são permitidas. Essas restrições, ou condições, variam em termos de complexidade e número de variáveis envolvidas. Compreender os tipos de condições é essencial para modelar problemas de forma eficiente e implementar soluções práticas.

### **Classificação dos Tipos de Condições**

#### **1. Condição Unitária**
- Uma condição unitária envolve **apenas uma variável**. 
- Ela restringe diretamente os valores que essa variável pode assumir.

**Exemplo Prático**:
- Em um problema de planejamento de rotas, uma condição unitária pode ser:  
  "O ponto de partida (cidade A) não pode ser G."  
  Formalmente: `A ≠ G`.

#### **2. Condição Binária**
- Uma condição binária envolve **duas variáveis** e especifica uma relação entre elas.
- Esse tipo de condição é comum em problemas que envolvem relações diretas entre pares de elementos.

**Exemplo Prático**:
- No problema do mapa das cores, onde regiões adjacentes devem ter cores diferentes:  
  "A cor de SA (South Australia) deve ser diferente da de NSW (New South Wales)."  
  Formalmente: `SA ≠ NSW`.

#### **3. Condição Ternária**
- Uma condição ternária envolve **três variáveis**, impondo uma relação entre todas elas.
- É útil quando há dependências mais complexas entre múltiplos elementos.

**Exemplo Prático**:
- No problema de alocação de assentos em uma conferência, onde assentos adjacentes seguem uma hierarquia de importância:  
  "A pessoa X deve ter uma posição superior à de Y, que, por sua vez, deve ser superior à de Z."  
  Formalmente: `X > Y > Z`.

#### **4. Condição Global**
- Uma condição global envolve **um número arbitrário de variáveis**, mas não necessariamente todas as variáveis do problema.
- Geralmente, são usadas em restrições amplas que aplicam a mesma regra para muitas variáveis ao mesmo tempo.

**Exemplo Prático**:
- No Sudoku, cada número deve aparecer apenas uma vez em cada linha, coluna e bloco 3x3. Isso pode ser representado pela condição global:  
  `Alldiff(A1, A2, A3, B1, B2, B3, C1, C2, C3)`.

### **Transformação de Condições**

Qualquer CSP pode ser convertido para um modelo contendo **apenas condições binárias**.  
Por exemplo:
- Uma condição ternária como `X > Y > Z` pode ser transformada em duas condições binárias:  
  `X > Y` e `Y > Z`.

Essa conversão simplifica a implementação de algoritmos, mas pode aumentar o número de condições no problema.


### **Condições de Preferência**

Além das condições necessárias, os CSPs também podem incluir **condições de preferência**. Essas condições não precisam ser satisfeitas, mas ajudam a encontrar soluções mais adequadas ou desejadas.

#### **Exemplo Prático: Agendamento Universitário**
1. **Condições Necessárias**:
   - Nenhum professor pode ministrar duas aulas ao mesmo tempo.
   - Nenhuma sala pode abrigar mais de uma turma simultaneamente.

2. **Condições de Preferência**:
   - O professor A prefere lecionar pela manhã.
   - A sala X deve ser usada prioritariamente para turmas com mais de 100 alunos.

Soluções que atendem às condições preferenciais são consideradas **soluções preferidas**, mesmo que não sejam obrigatórias.

### **Propagação de Condições**

A propagação de condições é uma técnica usada para reduzir o espaço de busca em CSPs, garantindo **consistência local**. Em vez de apenas tentar atribuições para as variáveis, o algoritmo:
1. Escolhe um valor para uma variável.
2. **Propaga a condição**, restringindo os valores possíveis das variáveis relacionadas.

Essa técnica:
- Elimina combinações inviáveis cedo no processo de busca.
- Pode disparar uma reação em cadeia, onde a redução em uma variável causa reduções em outras.

**Exemplo Prático: Sudoku**
- Ao atribuir o número 5 à célula A1, as condições do Sudoku eliminam 5 como opção para todas as outras células na mesma linha, coluna e bloco.

### **Conclusão**

Entender os tipos de condições e como aplicá-las em CSPs é crucial para resolver problemas complexos de forma eficiente. Enquanto as condições unitárias, binárias e ternárias são úteis para modelar relações simples, as condições globais e de preferência são fundamentais para problemas mais amplos e realistas.

---

## **Consistência em CSPs 
### Consistência de Nós**

A consistência de nós é o nível mais básico de consistência aplicado em problemas de satisfação de condições (CSPs). É um conceito que ajuda a simplificar o espaço de busca inicial, garantindo que cada variável seja compatível com suas condições unitárias antes de iniciar a resolução do problema. Isso é particularmente importante para reduzir os valores possíveis no domínio das variáveis, eliminando atribuições que violam as restrições básicas.


### **O que é Consistência de Nós?**

Uma variável (representada como um nó em um grafo que modela o CSP) é considerada **nó-consistente** quando **todos os valores do domínio dessa variável satisfazem suas condições unitárias**.

Se uma variável possui valores em seu domínio que não satisfazem as condições unitárias associadas a ela, esses valores podem ser eliminados sem afetar a solução final do problema.


### **Como Garantir a Consistência de Nós?**

A consistência de nós é geralmente alcançada como um passo de pré-processamento nos CSPs:
1. Para cada variável, avalie todas as condições unitárias associadas a ela.
2. Remova do domínio da variável todos os valores que violam essas condições.

### **Vantagens da Consistência de Nós**

1. **Redução do Espaço de Busca**:  
   Eliminar valores inválidos no início reduz significativamente o número de combinações a serem exploradas.

2. **Simplicidade**:  
   Consistência de nós é um conceito direto e pode ser aplicado de forma eficiente como uma etapa preliminar.

3. **Base para Outras Consistências**:  
   Garantir a consistência de nós é um pré-requisito para aplicar técnicas mais avançadas, como a consistência de arco e consistência de trajetória.


### **Exemplo de Consistência de Nós: Alocação de Tarefas**

Vamos aplicar o conceito de consistência de nós em um problema de **alocação de tarefas**. Imagine que temos um conjunto de funcionários e tarefas que devem ser atribuídas, com algumas condições específicas.


#### **Problema**

Temos três funcionários (**A**, **B**, **C**) que podem realizar tarefas específicas. O objetivo é garantir que cada funcionário receba uma tarefa que esteja dentro de sua capacidade.

1. **Variáveis**:
   - Funcionários: `A`, `B`, `C`.

2. **Domínios**:
   - Cada funcionário pode realizar as seguintes tarefas:  
     - `A`: `{T1, T2, T3}`  
     - `B`: `{T2, T3, T4}`  
     - `C`: `{T1, T3, T4}`

3. **Condições Unitárias**:
   - Funcionário `A` não pode realizar a tarefa `T2`.  
   - Funcionário `C` só pode realizar tarefas que terminam com o número `4`.  


#### **Garantindo Consistência de Nós**

1. Para a variável `A`, eliminamos `T2` do domínio, já que a condição unitária impede `A` de realizar essa tarefa.
2. Para a variável `C`, restringimos o domínio para conter apenas `T4`, pois essa é a única tarefa válida de acordo com a condição unitária.

#### **Novo Domínio Após Consistência de Nós**:
- `A`: `{T1, T3}`  
- `B`: `{T2, T3, T4}`  
- `C`: `{T4}`  

#### **Código em Python**

Aqui está a implementação do exemplo:

```python
from constraint import Problem

# Função para aplicar consistência de nós
def apply_node_consistency(problem, variable, condition):
    """
    Remove valores do domínio de uma variável que não satisfazem a condição.
    """
    domain = problem._variables[variable]
    consistent_domain = [value for value in domain if condition(value)]
    problem._variables[variable] = consistent_domain

# Exemplo: Alocação de tarefas
def task_allocation_example():
    problem = Problem()
    
    # Adicionando variáveis e seus domínios
    problem.addVariable("A", ["T1", "T2", "T3"])  # Funcionário A
    problem.addVariable("B", ["T2", "T3", "T4"])  # Funcionário B
    problem.addVariable("C", ["T1", "T3", "T4"])  # Funcionário C
    
    # Aplicando consistência de nó
    apply_node_consistency(problem, "A", lambda task: task != "T2")  # A não pode realizar T2
    apply_node_consistency(problem, "C", lambda task: task.endswith("4"))  # C só realiza tarefas que terminam com 4
    
    # Mostrando os domínios atualizados
    return problem._variables

# Executar exemplo
domains_after_consistency = task_allocation_example()
print("Domínios após consistência de nós:", domains_after_consistency)
```


#### **Saída**:
```
Domínios após consistência de nós: {'A': ['T1', 'T3'], 'B': ['T2', 'T3', 'T4'], 'C': ['T4']}
```

#### **Interpretação**

1. O domínio de `A` foi reduzido de `{T1, T2, T3}` para `{T1, T3}` devido à condição de que `A` não pode realizar `T2`.
2. O domínio de `C` foi reduzido de `{T1, T3, T4}` para `{T4}`, pois somente `T4` satisfaz a condição de tarefas que terminam com `4`.
3. O domínio de `B` permaneceu inalterado, pois nenhuma condição unitária foi aplicada a ele.


### **Resumo**

Este exemplo ilustra como aplicar consistência de nós em um problema realista de alocação de tarefas, mostrando como as condições unitárias ajudam a reduzir o espaço de busca e tornam o problema mais manejável antes de avançar para outras formas de consistência, como consistência de arcos.

---

### **Consistência de Arco**

A **consistência de arco** é um conceito crucial em problemas de satisfação de condições (CSPs), principalmente quando lidamos com relações entre duas variáveis. Esse tipo de consistência se concentra em garantir que, para todas as variáveis de um problema, cada valor no domínio de uma variável seja compatível com algum valor no domínio da variável relacionada por uma condição binária.


### **O que é Consistência de Arco?**

Uma **variável em um CSP** é considerada **arco-consistente** se **todos os valores em seu domínio** satisfazem **as condições binárias** associadas a ela. Isso significa que, para cada valor da variável, existe pelo menos um valor na variável relacionada que respeita a restrição entre as duas variáveis.

Um **grafo é arco-consistente** se todas as variáveis do grafo são arco-consistentes entre si, ou seja, se todas as condições binárias entre as variáveis são satisfeitas.

### **Exemplo de Consistência de Arco**

Vamos usar o exemplo de um **problema de alocação de horários para aulas**, em que temos duas variáveis, **Professor A** e **Professor B**, que precisam ser alocados a horários diferentes.

#### **Problema**:

Temos duas variáveis, **A** (para o Professor A) e **B** (para o Professor B). Ambas têm o seguinte domínio de valores, representando os horários disponíveis:

- **Domínio de A**: `{1, 2, 3}` (representando os horários 1, 2 e 3).
- **Domínio de B**: `{2, 3, 4}` (representando os horários 2, 3 e 4).

**Condição Binária**: **A** e **B** não podem ser alocados ao mesmo horário.

Portanto, a condição binária é: **A ≠ B** (A e B não podem ter o mesmo valor de horário).


#### **Consistência de Arco no Exemplo**

1. Para a variável **A** com domínio `{1, 2, 3}`, precisamos verificar se existe algum valor no domínio de **B** que seja diferente de **A** para cada valor de **A**.

2. Para **A = 1**, verificamos se existe algum valor em **B** que seja diferente de 1. Os valores válidos para **B** são `{2, 3, 4}`, e todos esses valores são diferentes de 1. Logo, **A = 1** é arco-consistente.

3. Para **A = 2**, verificamos se existe algum valor em **B** que seja diferente de 2. Os valores válidos para **B** são `{3, 4}`, ambos diferentes de 2, então **A = 2** é arco-consistente.

4. Para **A = 3**, verificamos se existe algum valor em **B** que seja diferente de 3. Os valores válidos para **B** são `{2, 4}`, ambos diferentes de 3, então **A = 3** também é arco-consistente.

5. Agora, para a variável **B**, precisamos verificar a consistência de arco em relação à variável **A**. Para cada valor no domínio de **B**, verificamos se existe algum valor no domínio de **A** que seja diferente de **B**.

6. Para **B = 2**, verificamos se existe algum valor em **A** que seja diferente de 2. O valor válido em **A** é 1, então **B = 2** é arco-consistente.

7. Para **B = 3**, verificamos se existe algum valor em **A** que seja diferente de 3. O valor válido em **A** é 1, então **B = 3** é arco-consistente.

8. Para **B = 4**, verificamos se existe algum valor em **A** que seja diferente de 4. Os valores válidos em **A** são `{1, 2, 3}`, todos diferentes de 4, então **B = 4** também é arco-consistente.


### **Resultado da Consistência de Arco**

- **A** é arco-consistente, pois para cada valor no seu domínio, existe pelo menos um valor em **B** que satisfaça a restrição **A ≠ B**.
- **B** é arco-consistente, pois para cada valor no seu domínio, existe pelo menos um valor em **A** que satisfaz a restrição **A ≠ B**.

Neste caso, a consistência de arco não reduz o número de valores nos domínios das variáveis, mas garante que todas as possíveis atribuições de valores entre as duas variáveis respeitem a restrição binária.


### **Aplicação do Algoritmo AC-3**

O algoritmo **AC-3 (Arc-Consistency 3)** é um dos métodos mais comuns para verificar a consistência de arco. Ele trabalha de forma iterativa, verificando se todas as restrições binárias são satisfeitas e ajustando os domínios das variáveis conforme necessário.

Em nosso exemplo, o algoritmo AC-3 aplicaria as verificações de consistência de arco para as variáveis **A** e **B** de acordo com a restrição **A ≠ B**. O algoritmo garantiria que os domínios de **A** e **B** permanecessem consistentes, sem a necessidade de fazer ajustes, pois o espaço de busca já está em conformidade com as restrições.


### **Exemplo em Código Python**

Aqui está a implementação de um exemplo simples de consistência de arco em Python:

```python
from constraint import Problem

# Função para aplicar consistência de arco
def arc_consistency(problem, var1, var2, condition):
    """
    Aplica consistência de arco entre duas variáveis.
    """
    domain1 = problem._variables[var1]
    domain2 = problem._variables[var2]
    
    # Verificar se todos os valores em var1 têm uma correspondência em var2 que satisfaça a condição
    for value1 in domain1[:]:
        valid = False
        for value2 in domain2:
            if condition(value1, value2):
                valid = True
                break
        if not valid:
            domain1.remove(value1)  # Eliminar valor inválido
    # Verificar também para var2
    for value2 in domain2[:]:
        valid = False
        for value1 in domain1:
            if condition(value1, value2):
                valid = True
                break
        if not valid:
            domain2.remove(value2)  # Eliminar valor inválido

# Exemplo: Alocação de horários para professores
def schedule_example():
    problem = Problem()
    
    # Adicionando variáveis e seus domínios
    problem.addVariable("A", [1, 2, 3])  # Professor A
    problem.addVariable("B", [2, 3, 4])  # Professor B
    
    # Aplicando consistência de arco
    arc_consistency(problem, "A", "B", lambda a, b: a != b)
    
    # Mostrando os domínios atualizados
    return problem._variables

# Executar exemplo
domains_after_arc_consistency = schedule_example()
print("Domínios após consistência de arco:", domains_after_arc_consistency)
```

#### **Saída**:

```
Domínios após consistência de arco: {'A': [1, 2, 3], 'B': [2, 3, 4]}
```


### **Conclusão**

A **consistência de arco** garante que, para cada par de variáveis, qualquer valor atribuído a uma variável tenha uma correspondente atribuição possível para a variável relacionada que satisfaça a restrição binária. Embora o exemplo mostrado aqui não tenha causado mudanças significativas nos domínios das variáveis, a consistência de arco é uma técnica crucial para reduzir o espaço de busca em problemas mais complexos. Além disso, algoritmos como **AC-3** são fundamentais para automatizar esse processo em problemas de maior escala.

---

### **Consistência de Trajeto**

A **consistência de trajeto** é um conceito mais avançado e menos intuitivo que a consistência de arco e nó. Ela aborda as relações entre **triplas de variáveis** em um **problema de satisfação de condições** (CSP). Ao contrário da consistência de arco, que se concentra em pares de variáveis, a consistência de trajeto examina a interação entre três variáveis e suas respectivas condições. 

### **O que é Consistência de Trajeto?**

A **consistência de trajeto** verifica se, dada uma **tripla de variáveis**, existe uma combinação de valores que satisfaça as condições de todas as três variáveis, levando em consideração as restrições entre essas variáveis. Essencialmente, a consistência de trajeto permite inferir novas condições implícitas observando interações de três variáveis ao mesmo tempo, e não apenas duas, como no caso da consistência de arco.

Uma tripla de variáveis **X**, **Y** e **Z** é considerada **trajeto-consistente** se, para cada atribuição de valores a **X** e **Y** que satisfaçam as condições entre **X** e **Y**, existir uma atribuição para **Z** que satisfaça tanto as condições entre **X** e **Z**, quanto entre **Y** e **Z**.

### **Alocação de Projetos a Funcionários**

Tendo um conjunto de **três funcionários** e **três projetos** que devem ser alocados. Cada projeto tem um prazo de entrega, e cada funcionário tem uma capacidade de trabalhar em determinados projetos dentro de uma janela de tempo. O problema é alocar os projetos aos funcionários, respeitando as condições de tempo e capacidade de cada um.

#### **Variáveis**:
- **Funcionário A**: Pode trabalhar em `{P1, P2, P3}`
- **Funcionário B**: Pode trabalhar em `{P1, P2, P3}`
- **Funcionário C**: Pode trabalhar em `{P1, P2, P3}`

#### **Restrições**:
- **P1** precisa ser completado até o dia 5.
- **P2** precisa ser completado até o dia 10.
- **P3** precisa ser completado até o dia 15.

**Condicionais**:
1. O **Funcionário A** pode trabalhar em **P1** e **P3**, mas não em **P2**.
2. O **Funcionário B** pode trabalhar em **P2** e **P3**, mas não em **P1**.
3. O **Funcionário C** pode trabalhar em **P1** e **P2**, mas não em **P3**.

A consistência de trajeto em relação a essas três variáveis (os funcionários A, B e C) verifica se a combinação de **A** e **B** que satisfaz suas restrições também permite uma alocação viável para o **Funcionário C**.

- Se, por exemplo, o **Funcionário A** for alocado em **P1** e o **Funcionário B** for alocado em **P2**, então a **consistência de trajeto** verifica se é possível alocar o **Funcionário C** em **P3** sem violar nenhuma restrição de prazo.

Em outras palavras, a consistência de trajeto exige que, para cada atribuição válida de **A** e **B**, exista uma atribuição válida para **C** que atenda todas as condições de capacidade e prazo.


### **Aplicação da Consistência de Trajeto**

A **consistência de trajeto** é especialmente útil quando lidamos com problemas que envolvem múltiplas interações entre variáveis, como em problemas de **planejamento de recursos** ou **alocação de tarefas**, onde é necessário garantir que a solução para um conjunto de variáveis seja consistente com outras variáveis relacionadas.

- **Em um problema de alocação de horários de aulas**, a consistência de trajeto pode ser usada para garantir que as condições de alocação de professores, salas e turmas sejam satisfatórias ao mesmo tempo. Por exemplo, se duas turmas estão programadas para usar a mesma sala, a consistência de trajeto pode ser utilizada para garantir que o horário de uma delas não entre em conflito com o horário da outra.

- **Em um problema de otimização de redes**, como alocação de recursos em redes de computadores, a consistência de trajeto pode ser usada para garantir que as variáveis associadas aos links de rede e aos dispositivos sejam combinadas de maneira que a rede funcione de maneira eficiente e sem congestionamentos.

### **Conclusão**

A **consistência de trajeto** é uma técnica poderosa e útil para resolver CSPs complexos, onde as restrições não envolvem apenas pares de variáveis, mas sim interações mais complexas entre trios de variáveis. Essa técnica permite inferir condições implícitas e reduzir o espaço de busca de maneira mais eficiente. Embora o conceito seja menos intuitivo que a consistência de arco e nó, ele é essencial para a resolução de problemas em áreas como alocação de recursos, planejamento de horários e otimização de redes.

---














### **Consistência k em CSPs**

A **consistência k** é uma extensão dos conceitos de consistência usados em problemas de satisfação de condições (CSPs), como **consistência de nó**, **consistência de arco** e **consistência de trajeto**. A ideia central por trás da consistência k é garantir que, para qualquer conjunto de (k - 1) variáveis, exista uma atribuição de valores consistente para a k-ésima variável, levando em consideração todas as restrições entre essas variáveis.

A consistência k permite que, ao considerar qualquer conjunto de (k - 1) variáveis em um CSP, se já houver uma atribuição consistente para essas (k - 1) variáveis, será possível encontrar uma atribuição válida para a k-ésima variável que satisfaça as condições do problema.

### **Definição Formal da Consistência k**

Uma CSP é considerada **k-consistente** se, para qualquer conjunto de (k - 1) variáveis e para qualquer atribuição consistente dessas variáveis, sempre é possível encontrar uma atribuição consistente para a k-ésima variável.

Além disso, uma CSP pode ser considerada **fortemente k-consistente** se ela for **k-consistente**, **(k-1)-consistente**, **(k-2)-consistente**, e assim por diante, até ser **1-consistente**. Ou seja, a consistência deve ser válida para todas as ordens de variáveis, desde o conjunto mais simples até o conjunto completo de variáveis do CSP.

### **Exemplo de Consistência 3**

Vamos ilustrar o conceito de consistência k com um exemplo que envolva alocação de tarefas em uma empresa, considerando a **consistência 3**.

#### **Cenário**

Uma empresa tem **3 departamentos** que precisam ser alocados em **3 salas** para realizar tarefas. Cada sala tem um conjunto específico de recursos, e as tarefas devem ser alocadas a essas salas de forma que nenhuma tarefa seja alocada a uma sala já ocupada por outra tarefa no mesmo período de tempo.

As variáveis no problema são os **departamentos** e as **salas**, e a restrição é que cada departamento deve ser alocado a uma sala única.

#### **Definição das Variáveis**:

- **Departamento 1**: {Sala A, Sala B, Sala C}
- **Departamento 2**: {Sala A, Sala B, Sala C}
- **Departamento 3**: {Sala A, Sala B, Sala C}

#### **Restrições**:

1. **Departamento 1** não pode ser alocado na mesma sala que **Departamento 2**.
2. **Departamento 2** não pode ser alocado na mesma sala que **Departamento 3**.
3. **Departamento 1** não pode ser alocado na mesma sala que **Departamento 3**.

Agora, vamos considerar que temos um conjunto de **3 variáveis** (os departamentos) e que desejamos verificar a **consistência 3** para este problema.

#### **Consistência 3**:

A consistência 3 exige que, para qualquer conjunto de 2 variáveis (digamos **Departamento 1** e **Departamento 2**), existirá uma atribuição para a **terceira variável** (o **Departamento 3**) que seja compatível com as atribuições já feitas para as duas primeiras variáveis.

- Se **Departamento 1** for alocado em **Sala A** e **Departamento 2** for alocado em **Sala B**, então, para que o problema seja consistente, **Departamento 3** deve ser alocado em **Sala C**. Esta alocação garante que todas as restrições sejam satisfeitas.

- Se **Departamento 1** for alocado em **Sala B** e **Departamento 2** for alocado em **Sala C**, então **Departamento 3** deve ser alocado em **Sala A**.

Portanto, para cada conjunto de **2 variáveis** já alocadas, sempre existirá uma alocação válida para o **Departamento 3**, o que demonstra que o problema é **3-consistente**.

### **Consistência Fortemente k-consistente**

Se a CSP for **fortemente k-consistente**, ela será **(k-1)-consistente**, **(k-2)-consistente**, até ser **1-consistente**. No exemplo acima, uma vez que a CSP seja 3-consistente, ela também seria automaticamente 2-consistente e 1-consistente, porque cada nível de consistência é garantido pela consistência do nível superior.

#### **Resumo do Tempo de Execução**

Em termos de **complexidade de tempo**, a consistência k pode ser computacionalmente cara, pois envolve verificar todas as combinações de (k - 1) variáveis e calcular as combinações consistentes para a k-ésima variável. Porém, a vantagem é que, ao garantir que todas as condições de consistência estão satisfeitas, é possível reduzir significativamente o espaço de busca e, portanto, otimizar a resolução do problema CSP.

### **Conclusão**

A **consistência k** é uma forma mais forte de garantir que uma CSP tenha uma solução válida, analisando conjuntos maiores de variáveis em relação às formas mais simples de consistência. Ela é particularmente útil quando o problema envolve interações complexas entre um número maior de variáveis. A consistência k permite uma verificação mais eficiente e pode simplificar a resolução do problema ao reduzir o espaço de busca, especialmente em CSPs com muitas variáveis inter-relacionadas.

---








### **Consistência Global em CSPs**

As **condições globais** são um conceito importante em problemas de satisfação de restrições (CSPs), onde um número arbitrário de variáveis pode ser envolvido em uma restrição. Ao contrário das condições unitárias, binárias ou ternárias, que envolvem um número fixo de variáveis, as condições globais podem envolver muitas variáveis ao mesmo tempo, o que traz desafios significativos para a solução de CSPs. A consistência global é uma forma de propagar essas condições e reduzir o espaço de busca de maneira eficiente, especialmente em casos em que múltiplas variáveis estão inter-relacionadas de maneira complexa.

A consistência global funciona de maneira semelhante à consistência de arco e de nó, mas com um nível mais alto de complexidade devido ao número maior de variáveis envolvidas. O objetivo é garantir que as condições globais sejam mantidas, propagando informações de maneira eficiente e verificando se é possível reduzir os domínios das variáveis de forma que todas as condições sejam satisfeitas simultaneamente.

### **Algoritmo de Consistência Global**

O algoritmo de consistência global se baseia em uma série de passos para propagação de informações e redução dos domínios das variáveis. O processo geral pode ser descrito da seguinte maneira:

1. **Remover variáveis com um único valor no domínio**: Se uma variável tem apenas um valor possível no seu domínio, esse valor é fixado e removido dos domínios das outras variáveis.
   
2. **Propagar a eliminação**: Se uma variável tem um valor fixado, esse valor deve ser removido dos domínios das variáveis que estão envolvidas nas condições globais. Isso pode reduzir os domínios de outras variáveis e simplificar o problema.

3. **Repetir o processo**: O processo é repetido para todas as variáveis cujos domínios tenham sido reduzidos a um único valor.

4. **Detectar inconsistências**: Se, em qualquer momento, o domínio de uma variável se tornar vazio ou se houver mais variáveis do que valores restantes, isso indica que uma inconsistência foi detectada e que não há solução possível para o problema.

5. **Propagação de limites**: Quando há um número grande de valores em um domínio, a propagação de limites é usada para restringir os valores possíveis sem verificar todas as combinações possíveis.

### **Exemplo de Condição Global:**

Considerando o cenário de um **agendamento de reuniões** em uma empresa, onde várias salas de conferências precisam ser alocadas para diferentes reuniões. Cada reunião tem um número específico de participantes, e as salas têm capacidades limitadas. Além disso, existem condições globais que envolvem múltiplas variáveis ao mesmo tempo.

#### **Cenário:**

- **Salas**: Sala A (capacidade 50 pessoas), Sala B (capacidade 100 pessoas), Sala C (capacidade 150 pessoas)
- **Reuniões**: Reunião 1 (precisa de entre 30 e 50 pessoas), Reunião 2 (precisa de entre 50 e 100 pessoas), Reunião 3 (precisa de entre 100 e 150 pessoas)

#### **Condicional Global:**

A condição global é que **nenhuma sala pode ser ocupada por duas reuniões simultaneamente**, e as **salas devem ser alocadas de forma que a soma das capacidades das reuniões não ultrapasse a capacidade total das salas**.

#### **Domínios Iniciais:**

- **Reunião 1**: {30, 50}
- **Reunião 2**: {50, 100}
- **Reunião 3**: {100, 150}
  
#### **Processo de Consistência Global:**

1. **Remover valores únicos**: Se uma reunião tem uma única possibilidade (como 50 pessoas na Reunião 1 ou 100 pessoas na Reunião 2), esse valor é fixado.

2. **Verificação da condição global**: Se as reuniões não podem ser alocadas simultaneamente em salas de conferência com capacidades superiores à soma das suas participações, vamos restringir os domínios. Por exemplo, se a Reunião 1 e a Reunião 2 forem alocadas em diferentes salas, a Reunião 3 precisa ser alocada em uma sala de maior capacidade, devido à sua maior demanda.

3. **Propagação de limites**: Se, por exemplo, a Reunião 1 está alocada na Sala A com 50 participantes, e a Reunião 2 está alocada na Sala B com 100 participantes, então a Reunião 3 deve ser alocada na Sala C, pois as outras salas já estão ocupadas. Isso restringe a possibilidade de alocar a Reunião 3 na Sala B ou A.

4. **Detecção de inconsistências**: Caso as condições não possam ser satisfeitas (por exemplo, se uma reunião não puder ser alocada a nenhuma sala devido a limitações de capacidade), uma inconsistência será detectada, e a solução será considerada inviável.

#### **Resultado:**

Após a aplicação da consistência global, as variáveis (reuniões) terão domínios de valores reduzidos e restringidos de forma a garantir que todas as condições sejam atendidas simultaneamente. Caso contrário, o algoritmo detectará uma inconsistência e ajustará os domínios para tentar encontrar uma solução viável.

### **Resumo**

A **consistência global** é uma abordagem importante para lidar com problemas complexos de CSPs que envolvem múltiplas variáveis e condições globais. Ela visa garantir que todas as condições sejam satisfeitas simultaneamente, propagando informações e restringindo os domínios das variáveis. Esse tipo de consistência é essencial para resolver problemas mais desafiadores, como alocação de recursos, planejamento e agendamento.

### **Referências**

1. Russell, S., & Norvig, P. (2021). **Artificial Intelligence: A Modern Approach** (4th Edition). Pearson.  
2. Dechter, R. (2003). **Constraint Processing**. Morgan Kaufmann.  
3. Apt, K. R. (2003). **Principles of Constraint Programming**. Cambridge University Press.  
4. Mackworth, A. K. (1977). "Consistency in Networks of Relations". *Artificial Intelligence*.  
5. Rossi, F., Van Beek, P., & Walsh, T. (2006). **Handbook of Constraint Programming**. Elsevier.