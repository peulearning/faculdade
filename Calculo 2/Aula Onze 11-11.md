# 🧮 Revisão — Cálculo Integral e Trigonometria

> _Anotações da aula e preparação para prova de Cálculo (Baseado em James Stewart, 9ª ed.)_

Nesta aula, foram revisados:

- 🧠 Dúvidas gerais sobre **integrais definidas e indefinidas**
- 📈 Interpretação de **gráficos de funções**
- 📘 Propriedades fundamentais de **funções pares e ímpares**
- 🔁 **Tabela de integrais imediatas**
- ⚙️ Aplicações físicas (posição, velocidade e aceleração)
- 📊 Questões de **verdadeiro ou falso**, com justificativas e contraexemplos

---

## ✨ Questões — Verdadeiro ou Falso (com análise)

### 1️⃣ Questão

Julgue as afirmações como **(V)** ou **(F)**, **justifique** as verdadeiras e **corrija ou apresente um contraexemplo** para as falsas:

---

#### a) 
> ( ) Se f(x) é uma função contínua tal que  
> ∫₁⁵ f(x) dx = 8 e ∫₃⁵ f(x) dx = 2,  
> então o valor de ∫₁³ f(x) dx é 10.

💡 **Dica de padrão:**

Quando os intervalos são contínuos e adjacentes, podemos usar a **adição de integrais**:

∫₁⁵ f(x) = ∫₁³ f(x) + ∫₃⁵ f(x)

Logo,  

8 = ∫₁³ f(x) + 2 → ∫₁³ f(x) = 6

✅ **Resposta:** Falsa. O correto é **6**, não **10**.

---

#### b)
> ( ) Se f(x) for uma função ímpar e contínua em [−a, a], então ∫₋ₐᵃ f(x) dx = 0

💡 **Dica de padrão:**

Função ímpar satisfaz f(−x) = −f(x).  
As áreas positivas e negativas se anulam simetricamente.

✅ **Resposta:** Verdadeira.

---

#### c)
> ( ) Se ∫ₐᵇ f(x) dx ≥ 0, então f(x) ≥ 0 para todo x em [a, b].

💡 **Dica de padrão:**

Uma integral positiva **não implica** que a função seja sempre positiva.  
Pode haver partes negativas compensadas por áreas maiores positivas.

⚠️ **Contraexemplo:**  

f(x) = sen(x) no intervalo [0, 2π] → ∫₀²π sen(x) dx = 0

➡️ **Falsa.**

---

#### d)
> ( ) ∫ x·cos(x²) dx = (x²/2)·sin(x²) + C

💡 **Dica de padrão:**

Sempre que o integrando tiver x·f’(x²), pense em **substituição**:

u = x² → du = 2x dx → (du/2) = x dx

Assim,  

∫ x·cos(x²) dx = (1/2)∫ cos(u) du = (1/2)·sin(u) + C = (1/2)·sin(x²) + C

❌ **Falsa.** (O resultado tem **1/2**, não **x²/2**.)

---

## 🔢 Integrais Importantes

| Expressão | Resultado | Dica |
|------------|------------|------|
| ∫ sec(x)·tan(x) dx | sec(x) + C | Derivada de sec(x) |
| ∫ cos(x) dx | sin(x) + C | Cosseno → Seno |
| ∫ sin(x) dx | −cos(x) + C | Seno → Cosseno negativo |
| ∫ x·eˣ dx | eˣ(x−1) + C | Integração por partes |
| ∫ 1/√(1−x²) dx | arcsin(x) + C | Tabela trigonométrica |

---

## 📘 Integrais Definidas (Treinadas em Aula)

### 🧩 Exemplo 1

**Calcular:** ∫ de 1/2 até √3/2 de 4 / √(1 − x²) dx

Reconheça o padrão:  
a derivada de arcsin(x) é 1 / √(1 − x²)

Logo:  
4·[arcsin(x)] de 1/2 até √1/2  
= 4(π/3 − π/6) = 2π/3

✅ **Resultado:** 2π/3

---

### 🧩 Exemplo 2

**Calcular:** ∫ de 1 a 4 de (2 + x) / x dx

Separando:  
∫ (2/x) dx + ∫ 1 dx

→ 2·ln|x| + x

Substituindo os limites:  
2(ln4 − ln1) + (4 − 1) = 2ln4 + 3

✅ **Resultado:** 2ln4 + 3

---

## 🎯 Dicas para Identificar Padrões

| Situação | Reconhecimento | Estratégia |
|-----------|----------------|-------------|
| Produto de x com uma função de x² | Substituição u = x² | Simplifica du |
| Função ímpar em [−a, a] | Área simétrica → 0 | Integral nula |
| Derivadas trigonométricas | sin ↔ cos ↔ sec² ↔ sec·tan | Memorizar tabela |
| Integração física (posição, vel., acel.) | v(t) = ∫a(t), s(t) = ∫v(t) | Aplicar constantes |
| Função sempre positiva → integral positiva | Mas o inverso nem sempre é verdade | Cuidado com áreas negativas |

---

## 🧠 Exercícios de Prática

- Calcule ∫ x·e^(x²) dx
    
- Determine ∫₀^(π/2) sin(x) dx
    
- Sendo f(x) = cos(x), avalie ∫₀^π f(x) dx
    
- Verifique se é verdadeira a afirmação:
    
    > “Se f(x) é contínua e ∫₋₃³ f(x) dx = 0, então f(x) é ímpar.”
    
- Resolva ∫ x / (1 − x²) dx

---
