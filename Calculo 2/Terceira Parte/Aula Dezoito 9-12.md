
# Aplicações: Áreas entre Curvas (Integração em dy)

## 1. O Conceito: Mudando a Perspectiva
Quando as curvas são funções de $y$ (ex: $x = y^2$) ou quando é difícil isolar o $y$, nós "fatiamos" a área **horizontalmente**.

* **Eixo de referência:** O eixo $Y$ (olhamos de baixo para cima para os limites).
* **Largura do retângulo:** $dy$.
* **Altura do retângulo:** (Função da Direita) - (Função da Esquerda).

$$
A = \int_{c}^{d} [f(y)_{\text{direita}} - g(y)_{\text{esquerda}}] \, dy
$$

> **Regra de Ouro:** "Direita menos Esquerda".
> * **Direita:** Valores de $x$ maiores.
> * **Esquerda:** Valores de $x$ menores.

---

## 2. Exemplo 6: A Parábola "Deitada"
**Problema:** Encontre a área entre a reta $y = x - 1$ e a parábola $y^2 = 2x + 6$.

### Passo 1: Isolar o X (Preparação)
Como temos um $y^2$ e um $y$, é mais fácil isolar o $x$ e integrar em $dy$.
1.  **Reta:** $y = x - 1 \Rightarrow \mathbf{x = y + 1}$ (Função Linear)
2.  **Parábola:** $y^2 = 2x + 6 \Rightarrow 2x = y^2 - 6 \Rightarrow \mathbf{x = \frac{1}{2}y^2 - 3}$

### Passo 2: Interseção (Limites de Integração)
Igualamos os $x$:
$$
y + 1 = \frac{1}{2}y^2 - 3
$$
Multiplique tudo por 2 para sumir com a fração:
$$
2y + 2 = y^2 - 6
$$
$$
y^2 - 2y - 8 = 0
$$
Fatorando (soma -2, produto -8): $(y - 4)(y + 2) = 0$.
**Limites:** $y = -2$ e $y = 4$.

### Passo 3: Esboço e Teste (Quem é Direita/Esquerda?)

* **Parábola:** Vértice em $x = -3$ (quando $y=0$). Abertura para a direita.
* **Reta:** Corta o eixo y em $-1$ e o eixo x em $1$.
* **Teste:** Escolha $y = 0$ (está entre -2 e 4).
    * Reta: $x = 0 + 1 = 1$.
    * Parábola: $x = -3$.
    * **Conclusão:** $1 > -3$, logo a **Reta está à Direita**.

### Passo 4: Integral
$$
A = \int_{-2}^{4} \left[ (y+1) - (\frac{1}{2}y^2 - 3) \right] \, dy
$$
Simplificando o integrando:
$$
A = \int_{-2}^{4} \left( -\frac{1}{2}y^2 + y + 4 \right) \, dy
$$

### Passo 5: Solução
$$
A = \left[ -\frac{y^3}{6} + \frac{y^2}{2} + 4y \right]_{-2}^{4}
$$
* Em $y=4$: $(-\frac{64}{6} + \frac{16}{2} + 16) = (-\frac{32}{3} + 8 + 16) = -\frac{32}{3} + 24 = \frac{40}{3}$
* Em $y=-2$: $(-(-\frac{8}{6}) + \frac{4}{2} - 8) = (\frac{4}{3} + 2 - 8) = \frac{4}{3} - 6 = -\frac{14}{3}$
* Total: $\frac{40}{3} - (-\frac{14}{3}) = \frac{54}{3} = \mathbf{18}$

---

## 3. Exemplo 7: Revisitando $y=x^2$ e $y=2x$
**Problema:** Área entre $y = x^2$ e $y = 2x$.

> **Nota de Estudo:** Já fizemos esse por $dx$ (vertical). Vamos fazer por $dy$ para treinar, pois dá o mesmo resultado.

### Passo 1: Isolar o X
1.  Parábola: $y = x^2 \Rightarrow x = \pm\sqrt{y}$. Como estamos no 1º quadrante (onde $2x$ encontra $x^2$), usamos $\mathbf{x = \sqrt{y}}$.
2.  Reta: $y = 2x \Rightarrow \mathbf{x = \frac{y}{2}}$.

### Passo 2: Interseção
$$
\sqrt{y} = \frac{y}{2} \Rightarrow y = \frac{y^2}{4} \Rightarrow 4y = y^2
$$
$$
y^2 - 4y = 0 \Rightarrow y(y-4) = 0
$$
**Limites:** $y = 0$ e $y = 4$.

### Passo 3: Quem é Direita/Esquerda?
Teste $y = 1$:
* Parábola: $x = \sqrt{1} = 1$.
* Reta: $x = 1/2 = 0.5$.
* **Conclusão:** A Parábola ($\sqrt{y}$) está à Direita (maior x).

### Passo 4: Integral e Solução
$$
A = \int_{0}^{4} \left( \sqrt{y} - \frac{y}{2} \right) \, dy = \int_{0}^{4} (y^{1/2} - \frac{1}{2}y) \, dy
$$
$$
A = \left[ \frac{2}{3}y^{3/2} - \frac{y^2}{4} \right]_0^4
$$
$$
A = \left( \frac{2}{3}(4)^{3/2} - \frac{16}{4} \right) - 0
$$
*Obs: $4^{3/2} = (\sqrt{4})^3 = 2^3 = 8$.*
$$
A = \frac{16}{3} - 4 = \frac{16 - 12}{3} = \mathbf{\frac{4}{3}}
$$

---

## 4. Exercícios de Treino (Método DY)

### Exercício 1: Reta e Parábola Horizontal
Encontre a área da região limitada por $x = y^2$ e $x = y + 2$.

> [!question]- Dica para Esboço
> * $x = y^2$: Parábola deitada, vértice na origem, abrindo para a direita.
> * $x = y + 2$: Reta que corta $y$ em $-2$ e $x$ em $2$.
> * Elas formam um "D" curvado.

> [!success]- Solução
> 1.  **Interseção:** $y^2 = y + 2 \Rightarrow y^2 - y - 2 = 0$.
>     $(y-2)(y+1) = 0$. Limites: $y = -1$ e $y = 2$.
> 2.  **Direita vs Esquerda:** Teste $y=0$.
>     * Parábola: $x=0$.
>     * Reta: $x=2$. (Reta é Direita).
> 3.  **Integral:**
>     $$\int_{-1}^{2} [(y+2) - y^2] \, dy$$
> 4.  **Cálculo:**
>     $$[\frac{y^2}{2} + 2y - \frac{y^3}{3}]_{-1}^{2}$$
>     * Em 2: $(2 + 4 - 8/3) = 10/3$.
>     * Em -1: $(1/2 - 2 + 1/3) = -7/6$.
>     * Total: $10/3 - (-7/6) = 20/6 + 7/6 = 27/6 = \mathbf{4.5}$

### Exercício 2: Duas Parábolas Deitadas
Encontre a área entre $x = -y^2 + 4y$ e $x = 0$ (Eixo Y).

> [!question]- Dica para Esboço
> * $x = -y^2 + 4y$: Parábola negativa (triste), abrindo para a esquerda? Não! Cuidado. Coeficiente negativo de $y^2$ abre para a **esquerda**.
> * Complete quadrados ou ache as raízes: $y(4-y)=0 \rightarrow y=0, y=4$.
> * Vértice em $y=2$.

> [!success]- Solução
> 1.  **Limites:** Os eixos interceptam em $y=0$ e $y=4$.
> 2.  **Direita vs Esquerda:**
>     * Curva: Em $y=2$, $x = -4 + 8 = 4$ (Positivo, está à direita).
>     * Eixo Y: $x = 0$ (Esquerda).
> 3.  **Integral:**
>     $$\int_{0}^{4} (-y^2 + 4y) \, dy$$
> 4.  **Cálculo:**
>     $$[-\frac{y^3}{3} + 2y^2]_0^4$$
>     $$(-64/3 + 32) - 0 = \frac{-64 + 96}{3} = \mathbf{\frac{32}{3}}$$