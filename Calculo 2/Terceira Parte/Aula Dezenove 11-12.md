	
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


---
## 🔄 Mais Exemplos Resolvidos

### Exemplo 2: Rotação no Eixo X (Mesma região anterior)

> [!EXAMPLE] Enunciado
> Calcule o volume da região delimitada por **$y = x^3$**, **$y = 8$** e **$x = 0$**, rotacionada agora em torno do **eixo X**.

**1. Análise Visual**
A região é a mesma, mas o eixo de rotação mudou. Agora a "fatia" é vertical ($dx$).
* A reta $y=8$ está "por cima" (mais longe do eixo X).
* A curva $y=x^3$ está "por baixo" (mais perto do eixo X).
* Isso cria um buraco no meio do sólido $\rightarrow$ **Método das Arruelas**.

**2. Limites de Integração ($dx$)**
Precisamos saber onde $y=x^3$ encontra $y=8$:
$$x^3 = 8 \implies x = 2$$
Limites: $a=0$ e $b=2$.

**3. Identificação dos Raios**
* **Raio Externo ($R$):** É a distância do eixo até o teto. $R = 8$.
* **Raio Interno ($r$):** É a distância do eixo até a curva. $r = x^3$.

**4. Montagem e Cálculo**
$$V = \pi \int_{0}^{2} (R^2 - r^2) \, dx$$
$$V = \pi \int_{0}^{2} (8^2 - (x^3)^2) \, dx$$
$$V = \pi \int_{0}^{2} (64 - x^6) \, dx$$

Integrando:
$$V = \pi \left[ 64x - \frac{x^7}{7} \right]_{0}^{2}$$
$$V = \pi \left( (128 - \frac{128}{7}) - 0 \right)$$
$$V = 128\pi \left( 1 - \frac{1}{7} \right) = 128\pi \left(\frac{6}{7}\right)$$

> [!SUCCESS] Resultado
> $$V = \frac{768\pi}{7} \, \text{u.v.}$$

---

### Exemplo 3: Intersecção de Curvas (Eixo X)

> [!EXAMPLE] Enunciado
> Volume da região entre **$y = x$** e **$y = x^2$** rotacionada em torno do **eixo X**.

**1. Pontos de Intersecção**
Igualamos as funções para achar os limites:
$$x = x^2 \implies x^2 - x = 0 \implies x(x-1)=0$$
Pontos: $x=0$ e $x=1$.

**2. Quem é quem? (No intervalo 0 a 1)**
Teste um valor, ex: $x=0,5$.
* $y = 0,5$ (Reta)
* $y = (0,5)^2 = 0,25$ (Parábola)
Logo, a **reta está por cima** da parábola.

**3. Raios (Arruela)**
* **Raio Maior ($R$):** $y = x$
* **Raio Menor ($r$):** $y = x^2$

**4. Cálculo**
$$V = \pi \int_{0}^{1} (x^2 - (x^2)^2) \, dx$$
$$V = \pi \int_{0}^{1} (x^2 - x^4) \, dx$$
$$V = \pi \left[ \frac{x^3}{3} - \frac{x^5}{5} \right]_{0}^{1}$$
$$V = \pi \left( \frac{1}{3} - \frac{1}{5} \right) = \pi \left( \frac{5-3}{15} \right)$$

> [!SUCCESS] Resultado
> $$V = \frac{2\pi}{15} \, \text{u.v.}$$

---

### Exemplo 4: Eixo de Rotação Deslocado ($y=2$)

> [!EXAMPLE] Enunciado
> Volume da região entre **$y = x$** e **$y = x^2$** rotacionada em torno da reta **$y = 2$**.

> [!DANGER] Atenção!
> O eixo de rotação não é mais o zero! O raio é a distância entre a curva e a reta $y=2$.
> A fórmula do raio muda para: $R = \text{Eixo} - \text{Curva}$ (ou vice-versa, o importante é a distância positiva).

**1. Análise Geométrica**
A região está entre $y=0$ e $y=1$. A reta $y=2$ está **acima** da região.
* A curva mais **longe** do eixo $y=2$ é a parábola $y=x^2$ (fundo).
* A curva mais **perto** do eixo $y=2$ é a reta $y=x$ (topo).

**2. Definindo os Raios**
* **Raio Externo ($R$):** Distância de $y=2$ até $y=x^2$.
  $$R = 2 - x^2$$
* **Raio Interno ($r$):** Distância de $y=2$ até $y=x$.
  $$r = 2 - x$$

**3. Montagem da Integral**
$$V = \pi \int_{0}^{1} ([2 - x^2]^2 - [2 - x]^2) \, dx$$

Expandindo os produtos notáveis:
1. $(2 - x^2)^2 = 4 - 4x^2 + x^4$
2. $(2 - x)^2 = 4 - 4x + x^2$

Subtraindo ($1 - 2$):
$$(4 - 4x^2 + x^4) - (4 - 4x + x^2) = x^4 - 5x^2 + 4x$$

**4. Resolução Final**
$$V = \pi \int_{0}^{1} (x^4 - 5x^2 + 4x) \, dx$$
$$V = \pi \left[ \frac{x^5}{5} - \frac{5x^3}{3} + \frac{4x^2}{2} \right]_{0}^{1}$$
$$V = \pi \left( \frac{1}{5} - \frac{5}{3} + 2 \right)$$

MMC entre 5 e 3 é 15:
$$V = \pi \left( \frac{3 - 25 + 30}{15} \right) = \pi \left( \frac{8}{15} \right)$$

> [!SUCCESS] Resultado
> $$V = \frac{8\pi}{15} \, \text{u.v.}$$


Além disso, estou resolvendo exercícios do livro e vídeos de playlists para me auxiliar.