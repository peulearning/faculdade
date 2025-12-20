---
tags: #cálculo #integrais #área #matemática
data: 2025-12-19
curso: Cálculo II
---

# Aplicações de Integrais: Áreas entre Curvas

## 1. Conceito Fundamental
Para encontrar a área $A$ de uma região delimitada por duas curvas $y = f(x)$ (curva superior) e $y = g(x)$ (curva inferior), e pelas linhas verticais $x = a$ e $x = b$, utilizamos a seguinte fórmula:

$$
A = \int_{a}^{b} [f(x) - g(x)] \, dx
$$

> **Regra de Ouro:** É sempre (Função de Cima) - (Função de Baixo). Se as curvas se cruzarem, é necessário dividir a integral nos pontos de interseção.

---

## 2. Exemplos de Aula

### Exemplo 1: Curvas Simples
**Problema:** Encontre a área da região limitada acima por $y = e^x$, abaixo por $y = x$, e limitada nos lados por $x = 0$ e $x = 1$.

**Passo a Passo:**
1. **Identificar limites:** O intervalo é $[0, 1]$.
2. **Identificar "Cima vs Baixo":** No intervalo $[0, 1]$, $e^x \ge x$.
3. **Montar a Integral:**
   $$
   A = \int_{0}^{1} (e^x - x) \, dx
   $$
4. **Resolver:**
   $$
   A = \left[ e^x - \frac{x^2}{2} \right]_{0}^{1}
   $$
   $$
   A = \left( e^1 - \frac{1}{2} \right) - \left( e^0 - 0 \right)
   $$
   $$
   A = e - 0.5 - 1 = \mathbf{e - 1.5}
   $$

---

### Exemplo 2: Interseção de Parábolas
**Problema:** Encontre a área da região delimitada pelas parábolas $y = x^2$ e $y = 2x - x^2$.

**Passo a Passo:**
1. **Encontrar Interseções (Limites de Integração):**
   Igualamos as funções: $x^2 = 2x - x^2 \Rightarrow 2x^2 - 2x = 0$.
   Fatorando: $2x(x - 1) = 0$.
   Pontos de interseção: $x = 0$ e $x = 1$.

2. **Teste de Região (Quem está por cima?):**
   Testando $x = 0.5$:
   * $y = x^2 \rightarrow 0.25$
   * $y = 2x - x^2 \rightarrow 1 - 0.25 = 0.75$ (Esta é a superior)

3. **Montar a Integral:**
   $$
   A = \int_{0}^{1} [(2x - x^2) - (x^2)] \, dx = \int_{0}^{1} (2x - 2x^2) \, dx
   $$

4. **Resolver:**
   $$
   A = \left[ x^2 - \frac{2x^3}{3} \right]_{0}^{1}
   $$
   $$
   A = \left( 1 - \frac{2}{3} \right) - (0) = \mathbf{\frac{1}{3}}
   $$

---

### Exemplo 3: Funções Trigonométricas (Cruzamento)
**Problema:** Encontre a área da região $y = \sin(x)$, $y = \cos(x)$, entre $x = 0$ e $x = \pi/2$.

**Análise:**
As funções seno e cosseno se cruzam em $x = \pi/4$. Isso divide a área em duas partes:
* **Parte 1 ($0$ a $\pi/4$):** $\cos(x) > \sin(x)$
* **Parte 2 ($\pi/4$ a $\pi/2$):** $\sin(x) > \cos(x)$

**Montagem:**
$$
A = \int_{0}^{\pi/4} (\cos x - \sin x) \, dx + \int_{\pi/4}^{\pi/2} (\sin x - \cos x) \, dx
$$

**Resolução:**
1. **Parte 1:**
   $[\sin x + \cos x]_0^{\pi/4} = (\frac{\sqrt{2}}{2} + \frac{\sqrt{2}}{2}) - (0 + 1) = \sqrt{2} - 1$
   
2. **Parte 2:**
   $[-\cos x - \sin x]_{\pi/4}^{\pi/2} = (-0 - 1) - (-\frac{\sqrt{2}}{2} - \frac{\sqrt{2}}{2}) = -1 + \sqrt{2}$

**Total:**
$$
A = (\sqrt{2} - 1) + (\sqrt{2} - 1) = \mathbf{2\sqrt{2} - 2}
$$