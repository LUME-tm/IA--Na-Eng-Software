# Análise de Código Java utilizando LLMs

## 1. Descrição da atividade

Esta atividade tem como objetivo utilizar diferentes **Large Language Models (LLMs)** para realizar a análise de um código-fonte Java.

Para isso, foi utilizado o **Ollama**, permitindo executar os modelos localmente e realizar perguntas sobre o código por meio de Python.

Os modelos utilizados foram:

* **DeepSeek-Coder**
* **StarCoder**
* **Qwen3-Coder**

A mesma pergunta e o mesmo código Java foram utilizados nos testes, permitindo comparar as respostas fornecidas por cada modelo.

---

## 2. Código Java analisado

O código utilizado para realizar os testes foi:

```java
public String getUserInitials(String firstName, String lastName) {
    return firstName.substring(0, 1).toUpperCase()
            + lastName.substring(0, 1).toUpperCase();
}
```

O método `getUserInitials` recebe o primeiro e o último nome de um usuário e retorna as iniciais em letras maiúsculas.

Por exemplo:

```text
firstName = "John"
lastName = "Smith"

Resultado: JS
```

---

# 3. Atividade 1 — DeepSeek-Coder

## Código utilizado

```python
from ollama import chat

cod="""
public String getUserInitials(String firstName, String lastName) {
    return firstName.substring(0, 1).toUpperCase()
            + lastName.substring(0, 1).toUpperCase();
}
"""

response = chat(
    model='deepseek-coder',
    messages=[{'role': 'user', 'content': 
    f"""
    existe algum problema neste codigo java?
    {cod}
    """}],
)

print(response.message.content)
```

## Pergunta realizada

> Existe algum problema neste código Java?

## Resposta do DeepSeek-Coder

> Não, o código é válido e não possui problemas técnicos. Utiliza o método `substring()` para obter as primeiras letras de `firstName` e `lastName`, e as chamam com `toUpperCase()` para que todas as letras no retorno fiquem em maiúscula.
>
> Por favor, não tenha dúvida se o método está funcionando conforme o esperado, pois esse é um código válido e não há necessidade de ajustes.

### Resultado

O **DeepSeek-Coder** considerou o código válido e não identificou problemas no código apresentado.

---

# 4. Atividade 2 — StarCoder

## Código utilizado

```python
from ollama import chat

cod="""
public String getUserInitials(String firstName, String lastName) {
    return firstName.substring(0, 1).toUpperCase()
            + lastName.substring(0, 1).toUpperCase();
}
"""

response = chat(
    model='StarCoder',
    messages=[{'role': 'user', 'content': 
    f"""
    existe algum problema neste codigo java?
    {cod}
    """}],
)

print(response.message.content)
```

## Pergunta realizada

> Existe algum problema neste código Java?

## Resposta do StarCoder

O StarCoder apresentou uma resposta inesperada. Em vez de analisar diretamente o código fornecido, começou a gerar diferentes versões do código e apresentou tokens relacionados à estrutura interna da conversa.

Entre os resultados apresentados estavam exemplos como:

```java
public String getAssistantInitials(String firstName, String lastName) {
    return firstName.substring(0, 1).toUpperCase()
            + lastName.substring(0, 1).toUpperCase();
}
```

Também foram geradas várias versões:

```java
public String getUser2Initials(String firstName, String lastName) {
    return firstName.substring(0, 1).toUpperCase()
             + lastName.substring(0, 1).toUpperCase();
}
```

```java
public String getAssistant2Initials(String firstName, String lastName) {
    return firstName.substring(0, 1).toUpperCase()
            + lastName.substring(0, 1).toUpperCase();
}
```

E posteriormente:

```java
public void showInitials() {
    System.out.println(getUserInitials(user.firstName, user.lastName));
    System.out.println(getAssistantInitials(assistant.firstName, assistant.lastName));
    System.out.println(getUser2Initials(user2.firstName, user2.lastName));
    System.out.println(getAssistant2Initials(assistant2.firstName, assistant2.lastName));
    System.out.println(getUser3Initials(user3.firstName, user3.lastName));
    System.out.println(getAssistant3Initials(assistant3.firstName, assistant3.lastName));
    System.out.println(getUser4Initials(user4.firstName, user4.lastName));
    System.out.println(getAssistant4Initials(assistant4.firstName, assistant4.lastName));
}
```

### Resultado

O **StarCoder não realizou a análise esperada**. Em vez de responder objetivamente se existia algum problema no código Java, apresentou repetições, alterações no código e tokens como:

```text
<|im_end|>
<|im_start|>user2
<|im_end|>
```

Portanto, nesse teste, a resposta do StarCoder não foi adequada para a atividade proposta.

---

# 5. Atividade 3 — Qwen3-Coder

## Código utilizado

```python
from ollama import chat

cod="""
public String getUserInitials(String firstName, String lastName) {
    return firstName.substring(0, 1).toUpperCase()
            + lastName.substring(0, 1).toUpperCase();
}
"""

response = chat(
    model='qwen3-coder',
    messages=[{'role': 'user', 'content': 
    f"""
    existe algum problema neste codigo java?
    {cod}
    """}],
)

print(response.message.content)
```

## Pergunta realizada

> Existe algum problema neste código Java?

## Resposta do Qwen3-Coder

O Qwen3-Coder identificou problemas relacionados ao tratamento de valores `null` e strings vazias.

### Problemas identificados

**1. Valor `null`**

Se `firstName` ou `lastName` for `null`, o código poderá gerar uma `NullPointerException`.

Exemplo:

```java
getUserInitials(null, "Smith");
```

**2. String vazia**

Se uma das strings estiver vazia, a chamada:

```java
substring(0, 1)
```

poderá gerar uma exceção.

Exemplo:

```java
getUserInitials("", "Smith");
```

O modelo sugeriu uma implementação com tratamento desses casos:

```java
public String getUserInitials(String firstName, String lastName) {
    if (firstName == null || firstName.isEmpty()) {
        firstName = "";
    }

    if (lastName == null || lastName.isEmpty()) {
        lastName = "";
    }
    
    return (firstName.length() > 0 ? firstName.substring(0, 1).toUpperCase() : "")
            + (lastName.length() > 0 ? lastName.substring(0, 1).toUpperCase() : "");
}
```

Também foi sugerida uma alternativa utilizando `Optional`:

```java
public String getUserInitials(String firstName, String lastName) {
    return Optional.ofNullable(firstName)
            .filter(s -> !s.isEmpty())
            .map(s -> s.substring(0, 1).toUpperCase())
            .orElse("")
            + Optional.ofNullable(lastName)
            .filter(s -> !s.isEmpty())
            .map(s -> s.substring(0, 1).toUpperCase())
            .orElse("");
}
```

### Resultado

O **Qwen3-Coder** apresentou a análise mais completa entre os três testes, identificando situações que poderiam causar exceções durante a execução do programa e sugerindo formas de tratamento.

---

# 6. Comparação dos resultados

| LLM                | Resultado da análise                             | Identificou problemas?    |
| ------------------ | ------------------------------------------------ | ------------------------- |
| **DeepSeek-Coder** | Considerou o código válido                       | Não                       |
| **StarCoder**      | Não realizou a análise corretamente              | Não foi possível concluir |
| **Qwen3-Coder**    | Identificou problemas de `null` e strings vazias | Sim                       |

---

# 7. Conclusão

A atividade demonstrou que diferentes LLMs podem apresentar comportamentos distintos quando recebem a mesma tarefa de análise de código.

O **DeepSeek-Coder** respondeu que o código estava correto, porém não considerou os casos em que os parâmetros poderiam ser `null` ou strings vazias.

O **StarCoder** apresentou uma resposta inadequada para o objetivo da atividade, gerando alterações e repetições do código em vez de realizar uma análise objetiva.

Já o **Qwen3-Coder** apresentou uma análise mais detalhada, identificando possíveis exceções relacionadas aos parâmetros recebidos pelo método e sugerindo soluções para tratar esses casos.

Dessa forma, para o exemplo analisado, o **Qwen3-Coder apresentou a resposta mais útil para a atividade de revisão de código**.

## 8. Tecnologias utilizadas

* Python
* Ollama
* DeepSeek-Coder
* StarCoder
* Qwen3-Coder
* Java

## 9. Objetivo final

O experimento demonstra como diferentes modelos de inteligência artificial podem ser utilizados como ferramentas de **revisão e análise de código**, permitindo comparar a capacidade de cada LLM em:

* Identificar erros;
* Encontrar possíveis exceções;
* Avaliar problemas no código;
* Sugerir melhorias;
* Explicar o funcionamento de um programa.
