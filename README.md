# LLM Eval Framework — trade Rules v2

Avalia a capacidade do Gemini seguir regras de trading em linguagem natural,
comparando 3 abordagens de prompt.

## Instalação

```bash
pip install google-genai
```

## Configuração

```bash
export GEMINI_API_KEY="a-tua-key"
```

Ou edita `GEMINI_API_KEY` directamente no script.

## Execução

```bash
python evaluator.py
```

## O que produz

- Output no terminal com ✓/✗ por cenário
- Relatório final com precisão por prompt e erros por regra
- CSV com todos os resultados: `eval_results_YYYYMMDD_HHMM.csv`

## Prompts comparados

| Prompt | Descrição |
|---|---|
| `direto` | Regras + dados → responde só com a acção |
| `chain_of_thought` | Força raciocínio passo a passo antes de decidir |
| `few_shot` | Regras + 7 exemplos concretos |

## Parâmetros ajustáveis

```python
SCENARIOS_COUNT = 50        # número de cenários
DELAY_BETWEEN_CALLS = 1.5   # segundos entre chamadas (evita rate limit)
MODEL = "gemini-2.5-flash-lite"
```

## Interpretar os resultados

- **Erros em R1/R2** → o modelo ignora condições de segurança simples
- **Erros em R5 vs R3** → o modelo não respeita prioridade entre regras
- **Erros em R7** → dificuldade com condições compostas (EMA + ROE)
- **chain_of_thought > direto** → confirma que scaffold cognitivo ajuda
- **few_shot > direto** → exemplos compensam raciocínio
