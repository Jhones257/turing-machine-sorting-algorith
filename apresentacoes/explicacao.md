# Explicação do Código da Máquina de Turing para Bubble Sort com Animação

Este código implementa uma simulação de uma Máquina de Turing para ordenar uma sequência de números utilizando o algoritmo de **Bubble Sort** e gera uma animação do processo passo a passo. A animação é feita utilizando `matplotlib.animation` e pode ser visualizada diretamente no Jupyter ou Google Colab. A seguir, explico as partes principais do código.

## 1. **Importação de Bibliotecas**

```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from IPython.display import HTML
```

- `matplotlib.pyplot`: Biblioteca usada para criar gráficos, responsável por exibir a animação.
- `matplotlib.animation`: Usada para gerar animações no `matplotlib`.
- `IPython.display.HTML`: Permite exibir a animação interativa no Jupyter/Colab.

## 2. **Definição da Classe `MaquinaDeTuring`**

A classe `MaquinaDeTuring` simula uma máquina de Turing que implementa o algoritmo **Bubble Sort**.

### 2.1 **Inicialização (`__init__`)**

```python
def __init__(self, fita):
    """
    Inicializa a Máquina de Turing com a fita de entrada.
    Converte os elementos da fita para inteiros e adiciona um espaço extra no final.
    """
    self.fita = [int(x) for x in fita] + [0]
    self.cabeca = 0  # Define a posição inicial da cabeça de leitura
    self.estado = 'inicio'  # Define o estado inicial da máquina
    self.flag_troca = 0  # Indica se houve troca na última passagem
    self.historico = []  # Registra o histórico de ações
    self.contador_passos = 0  # Contador de passos executados
    self.passos_executados = []  # Lista para armazenar os estados da fita para animação
```

- **`fita`**: Converte a lista de números da fita em inteiros e adiciona um valor extra (`0`) ao final, representando o espaço vazio.
- **`cabeca`**: A posição inicial da cabeça de leitura é 0.
- **`estado`**: Define o estado inicial da máquina de Turing (`'inicio'`).
- **`flag_troca`**: Flag que indica se houve troca durante o passo anterior.
- **`historico`**: Registra o histórico das ações da máquina.
- **`contator_passos`**: Conta o número de passos executados.
- **`passos_executados`**: Armazena os quadros da animação (fita, cabeça, estado, etc.).

### 2.2 **Registrar Ação**

```python
def registrar_acao(self, acao):
    """
    Registra uma ação da Máquina de Turing no histórico.
    Salva o estado atual da fita e da cabeça de leitura.
    """
    self.contador_passos += 1
    log = f"Passo {self.contador_passos}: {acao} | Flag de Troca: {self.flag_troca}"
    self.historico.append(log)
    self.passos_executados.append((self.fita[:], self.cabeca, self.estado, self.flag_troca, log))
```

A função `registrar_acao` registra o estado atual da fita e da cabeça, além da ação realizada, para posterior uso na animação.

### 2.3 **Execução de um Passo**

```python
def passo(self):
    """
    Executa um único passo da Máquina de Turing.
    Realiza comparações, trocas e movimentação da cabeça de leitura.
    """
    if self.estado == 'parado':
        return

    simbolo_atual = self.fita[self.cabeca]  # Obtém o valor na posição atual da cabeça
```

O método `passo` executa uma única iteração de acordo com o estado atual da máquina de Turing. Ele passa por diferentes estados, realizando comparações e trocas no algoritmo Bubble Sort.

### 2.4 **Estados e Ações**

- **'inicio'**: Reinicia o processo, movendo a cabeça para a posição 0.
- **'comparar'**: Compara dois números adjacentes. Se a troca for necessária, muda para o estado `'trocar'`.
- **'trocar'**: Realiza a troca dos números adjacentes.
- **'verificar_flag'**: Verifica se houve troca. Se sim, reinicia o processo; caso contrário, a ordenação está concluída.

### 2.5 **Execução Completa**

```python
def executar(self):
    """
    Executa a Máquina de Turing até que o estado seja 'parado'.
    Retorna a fita ordenada e o histórico de ações.
    """
    while self.estado != 'parado':
        self.passo()
    return self.fita[:-1], self.historico  # Retorna a fita ordenada e o histórico de ações
```

O método `executar` executa a máquina até que o estado seja `'parado'`, retornando a fita ordenada e o histórico de ações.

## 3. **Configuração da Animação**

A animação é configurada para mostrar a execução da máquina de Turing. Para isso, utilizamos `matplotlib` e `FuncAnimation` para atualizar o gráfico a cada passo da execução.

### 3.1 **Função de Atualização**

```python
def atualizar(frame):
    """
    Atualiza o gráfico da animação para representar um quadro específico do processo.
    """
    fita, cabeca, estado, flag_troca, log = quadros[frame]
    ax.clear()
    ax.set_title(f"Máquina de Turing - Bubble Sort - Passo {frame+1}")
    ...
```

A função `atualizar` é chamada para atualizar a animação a cada passo. Ela desenha a fita, o estado atual da máquina e outras informações no gráfico.

### 3.2 **Criação da Animação**

```python
animacao_mt = animation.FuncAnimation(fig, atualizar, frames=num_frames, interval=500, repeat=False)
HTML(animacao_mt.to_jshtml())
```

A animação é criada com `FuncAnimation`, passando o número de quadros (`num_frames`) e o intervalo entre cada quadro. O método `to_jshtml()` converte a animação para o formato HTML, permitindo visualizá-la no Jupyter/Colab.

## 4. **Execução Final**

```python
fita_inicial = ['1', '4', '4', '3', '2']
mt = MaquinaDeTuring(fita_inicial)
mt.executar()
quadros = mt.passos_executados  # Armazena os quadros para a animação
```

- **Fita Inicial**: A fita de entrada é definida com os números a serem ordenados.
- **Execução da Máquina**: A máquina de Turing é executada e a animação é criada a partir dos quadros registrados.

---

Este código simula a Máquina de Turing executando o algoritmo **Bubble Sort** e exibe visualmente o processo de ordenação, o que ajuda a entender o funcionamento da máquina de Turing de forma interativa e animada.
