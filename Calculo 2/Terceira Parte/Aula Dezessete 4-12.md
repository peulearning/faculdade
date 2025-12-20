
*Página 437 PDF*
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

---
## 4. Algoritmo de Resolução (Passo a Passo)

Para não travar na hora de resolver, siga este roteiro mental. O desenho é **obrigatório** para não errar o sinal da integral.

1.  **O Esboço é Rei:**
    * Desenhe as curvas no plano cartesiano. Não precisa ser perfeito, apenas o suficiente para ver o comportamento.
    * *Dica:* Se não souber desenhar de cabeça, plote alguns pontos (x=0, 1, -1...).

2.  **Identifique as Fronteiras (Limites de Integração):**
    * Se o problema não der os intervalos (ex: "entre x=0 e x=1"), você precisa achar onde as curvas se cruzam.
    * **Como:** Iguale as funções ($f(x) = g(x)$) e resolva para $x$.

3.  **Defina o "Teto" e o "Chão":**
    * Olhe para o seu desenho dentro do intervalo escolhido.
    * Qual função toca o "céu"? Essa é a $f(x)$ (vem primeiro).
    * Qual função toca o "chão"? Essa é a $g(x)$ (subtrai).
    * *Atenção:* Se elas trocarem de lugar no meio do caminho (como no exemplo do seno e cosseno), você deve dividir em duas integrais.

4.  **Monte e Integre:**
    * Aplique a fórmula: $\int_{a}^{b} [\text{Teto} - \text{Chão}] \, dx$.

---

## 5. Exercícios de Fixação

Tente resolver desenhando o gráfico primeiro. Clique na seta "Solução" para conferir.

### Exercício A: Parábola e Reta
Encontre a área da região delimitada por $y = x^2$ e $y = 4$.

> [!question]- Dica do Gráfico
> É uma parábola virada para cima ($x^2$) cortada por uma linha horizontal na altura 4.

> [!success]- Solução Passo a Passo
> 1. **Interseção:** $x^2 = 4 \Rightarrow x = \pm 2$. Limites: $[-2, 2]$.
> 2. **Teto vs Chão:** A reta $y=4$ está acima da parábola $y=x^2$ nesse intervalo.
> 3. **Integral:**
>    $$
>    A = \int_{-2}^{2} (4 - x^2) \, dx
>    $$
>    Como a função é par e o intervalo simétrico, podemos calcular de 0 a 2 e multiplicar por 2 (opcional, mas facilita):
>    $$
>    A = 2 \int_{0}^{2} (4 - x^2) \, dx = 2 \left[ 4x - \frac{x^3}{3} \right]_0^2
>    $$
>    $$
>    A = 2 \left( (8 - \frac{8}{3}) - 0 \right) = 2 \left( \frac{24-8}{3} \right) = \mathbf{\frac{32}{3}}
>    $$

---

### Exercício B: Duas Parábolas
Encontre a área da região entre $y = x^2 - 2x$ e $y = 4 - x^2$.

> [!question]- Dica do Gráfico
> $y = x^2 - 2x$ é uma parábola "sorrindo" (côncava para cima).
> $y = 4 - x^2$ é uma parábola "triste" (côncava para baixo).
> Elas vão criar uma região "oval" fechada entre elas.

> [!success]- Solução Passo a Passo
> 1. **Interseção:**
>    $x^2 - 2x = 4 - x^2$
>    $2x^2 - 2x - 4 = 0$ (dividindo tudo por 2...)
>    $x^2 - x - 2 = 0 \Rightarrow (x-2)(x+1) = 0$.
>    Limites: $x = -1$ e $x = 2$.
> 2. **Teto vs Chão:** Para saber quem está em cima, teste $x=0$ (que está entre -1 e 2).
>    * $y = 0^2 - 0 = 0$
>    * $y = 4 - 0^2 = 4$ (Logo, $4-x^2$ é a função de cima).
> 3. **Integral:**
>    $$
>    A = \int_{-1}^{2} [(4 - x^2) - (x^2 - 2x)] \, dx
>    $$
>    Organizando o integrando:
>    $$
>    A = \int_{-1}^{2} (-2x^2 + 2x + 4) \, dx
>    $$
> 4. **Resolvendo:**
>    $$
>    A = \left[ -\frac{2x^3}{3} + x^2 + 4x \right]_{-1}^{2}
>    $$
>    Substituindo 2: $(-\frac{16}{3} + 4 + 8) = -\frac{16}{3} + 12 = \frac{20}{3}$
>    Substituindo -1: $(\frac{2}{3} + 1 - 4) = \frac{2}{3} - 3 = -\frac{7}{3}$
>    Final: $\frac{20}{3} - (-\frac{7}{3}) = \frac{27}{3} = \mathbf{9}$

---

### Exercício C: Função Raiz (Cuidado com o desenho)
Encontre a área limitada por $y = \sqrt{x}$ e $y = \frac{x}{2}$.

> [!question]- Dica do Gráfico
> A função raiz começa na origem e cresce curvando para a direita.
> A função $x/2$ é uma reta que passa na origem.
> Elas se tocam na origem e em outro ponto mais à frente.

> [!success]- Solução Passo a Passo
> 1. **Interseção:**
>    $\sqrt{x} = \frac{x}{2} \Rightarrow x = \frac{x^2}{4} \Rightarrow x^2 = 4x$.
>    $x^2 - 4x = 0 \Rightarrow x(x-4) = 0$.
>    Limites: $x = 0$ e $x = 4$.
> 2. **Teto vs Chão:** No intervalo $(0,4)$, teste $x=1$:
>    * $\sqrt{1} = 1$
>    * $1/2 = 0.5$ (Logo, a Raiz é o teto).
> 3. **Integral:**
>    $$
>    A = \int_{0}^{4} (\sqrt{x} - \frac{x}{2}) \, dx = \int_{0}^{4} (x^{1/2} - \frac{1}{2}x) \, dx
>    $$
> 4. **Resolvendo:**
>    $$
>    A = \left[ \frac{2}{3}x^{3/2} - \frac{1}{2} \cdot \frac{x^2}{2} \right]_0^4
>    $$
>    $$
>    A = \left[ \frac{2}{3}(\sqrt{4})^3 - \frac{1}{4}(16) \right] - 0
>    $$
>    $$
>    A = \frac{2}{3}(8) - 4 = \frac{16}{3} - \frac{12}{3} = \mathbf{\frac{4}{3}}
>    $$