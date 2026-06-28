# Financial Decision AI: MDP & Reinforcement Learning

Este repositório contém a implementação de agentes inteligentes para tomada de decisão em mercados financeiros simulados, utilizando Processos de Decisão de Markov (MDP). O projeto compara abordagens de **Planejamento (Model-Based)** via Programação Dinâmica e **Aprendizado por Reforço (Model-Free)** via Q-Learning, documentado no relatório `Relatorio.pdf`.

## Arquitetura do Projeto

O projeto é modularizado para separar a lógica de ambiente, agentes, utilitários e scripts de execução:

### 1. Agentes e Lógica de Decisão
* `agent/q_learning.py`: Implementação do agente de Q-Learning com suporte a estratégias de exploração ($\epsilon$-greedy).
* `agent/policy_evaluation.py`: Implementação da classe `ModelBasedEvaluator`, responsável pela construção do modelo empírico de transição e resolução das equações de Bellman.

### 2. Scripts de Treinamento e Execução
* `train.py`: Script principal para treinar o agente de Q-Learning em múltiplos estados (Simples, Janela, Médias Móveis) e gerar gráficos de performance.
* `train_value_iteration.py`: Executa o planejamento via Value Iteration para encontrar a política ótima teórica e gera mapas de calor (`heatmap`) de valores.
* `comparar_algoritmos.py`: Realiza o *benchmark* direto entre o desempenho do Q-Learning e o Value Iteration (Bellman).

### 3. Análise e Otimização
* `otimizar_hiperparametros.py`: Script de *Grid Search* que testa diversas combinações de $\alpha$ (taxa de aprendizado) e $\gamma$ (fator de desconto), gerando um mapa de calor da lucratividade.
* `testar_exploracao.py`: Compara estratégias de exploração, especificamente o decaimento de epsilon (`epsilon_decay`) versus exploração fixa.

## Metodologia Técnica
O problema foi formalizado como um MDP $(S, A, T, R)$:
* **Espaço de Estados ($S$)**: Discretizado em três modelos: 
    1. **Simples**: 1 dia de preço.
    2. **Janela**: 3 dias de tendência.
    3. **Médias Móveis**: Cruzamento de 5 e 20 períodos.
* **Função de Recompensa ($R$)**: Baseada na variação líquida do portfólio.
* **Convergência**: O Value Iteration utiliza o limiar $\theta=10^{-5}$ para garantir a estabilidade teórica da função de valor $V(s)$.

## Resultados Principais
* **Model-Based vs Model-Free**: O Value Iteration provou ser o "teto" de lucro possível ($9421.95), enquanto o Q-Learning atingiu uma performance competitiva ($9231.78), validando a robustez do aprendizado sem necessidade de modelo prévio.
* **Hiperparâmetros**: A análise do Grid Search confirmou que baixos valores de $\alpha$ são essenciais para filtrar o ruído do mercado simulado, enquanto valores elevados de $\gamma$ incentivam a priorização de ganhos de longo prazo.

## Como Executar

   ```bash
   pip install numpy matplotlib seaborn

   # Treinar Q-Learning (Model-Free)
    python train.py

    # Executar Planejamento (Model-Based)
    python train_value_iteration.py

    # Comparar os dois algoritmos
    python comparar_algoritmos.py
```
## Equipe
Trabalho desenvolvido pelos alunos:
* Bruno Jambeiro Mesquita
* Lucas Rodrigues de Mendonça
* Fernando Rodrigues da Silva
* Thiago Augusto de Tulio Nascimento
* Victor Itiro Ogitsu