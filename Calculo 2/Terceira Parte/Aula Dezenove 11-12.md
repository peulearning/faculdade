
# 🧊 Volume de Sólidos de Revolução

> [!SUMMARY] Conceito Fundamental
> O volume de um sólido de revolução é calculado integrando a área da secção transversal $A(x)$ ao longo de um intervalo $[a, b]$.
> 
> $$V = \int_{a}^{b} A(x) \, dx$$
> *(Nota: Se a rotação for no eixo y, integra-se em $dy$)*

---

## 📐 Métodos de Cálculo

Existem duas situações principais dependendo da geometria do sólido gerado:

### 1. Método dos Discos (Círculo Sólido)
Utilizado quando o sólido **não possui buracos**. A secção transversal é um círculo perfeito.

> [!tip] Fórmula: Disco
> $$A(r) = \pi r^2$$
> $$V = \pi \int_{a}^{b} [f(x)]^2 \, dx$$

### 2. Método das Arruelas (Coroa Circular)
Utilizado quando o sólido **possui um buraco** no meio. A secção transversal é um anel.

> [!warning] Fórmula: Coroa Circular
> $$A(R, r) = \pi (R^2 - r^2)$$
> $$V = \pi \int_{a}^{b} ([R(x)]^2 - [r(x)]^2) \, dx$$
> Onde:
> * $R$ = Raio Externo (função mais longe do eixo)
> * $r$ = Raio Interno (função mais perto do eixo)

---

## 📝 Algoritmo de Resolução (Passo-a-Passo)

Para não errar, siga este checklist lógico:

1.  **Esboçar a Região:** Desenhe as curvas e identifique a área que será rotacionada.
2.  **Identificar o Eixo:** Determine se a rotação é em $x$ ($dx$) ou em $y$ ($dy$).
3.  **Definir Limites ($a$ e $b$):** Encontre os pontos de intersecção das curvas.
4.  **Determinar os Raios:**
    * Se for **Disco**: Quem é o raio $r$?
    * Se for **Coroa**: Quem é o raio maior $R$ e o raio menor $r$?
5.  **Montar e Resolver:** Aplique a integral.

---

## 📚 Exemplo Prático

> [!EXAMPLE] Enunciado
> Calcule o volume gerado pela rotação da região delimitada por **$y = x^3$**, **$y = 8$** e **$x = 0$** em torno do **eixo Y**.

### Resolução Guiada

**1. Análise das Funções e Eixo**
Como a rotação é no **Eixo Y**, precisamos integrar em relação a $dy$.
Logo, precisamos isolar o $x$ na função:
$$y = x^3 \implies x = \sqrt[3]{y} = y^{\frac{1}{3}}$$

**2. Limites de Integração**
A região vai de $y=0$ (dado por $x=0$ na origem) até $y=8$.
* $a = 0$
* $b = 8$

**3. Identificação do Raio**
O sólido não tem buraco no meio (está encostado no eixo y). Usamos o método do **Círculo (Disco)**.
* Raio $r = x = y^{\frac{1}{3}}$

**4. Montagem da Integral**
$$V = \pi \int_{0}^{8} (y^{\frac{1}{3}})^2 \, dy$$
$$V = \pi \int_{0}^{8} y^{\frac{2}{3}} \, dy$$

**5. Cálculo Final**
$$V = \pi \left[ \frac{3}{5} y^{\frac{5}{3}} \right]_{0}^{8}$$
$$V = \frac{3\pi}{5} (8^{\frac{5}{3}} - 0)$$
*(Sabemos que $\sqrt[3]{8} = 2$, e $2^5 = 32$)*
$$V = \frac{3\pi}{5} (32)$$

> [!SUCCESS] Resultado
> $$V = \frac{96\pi}{5} \, \text{u.v.}$$