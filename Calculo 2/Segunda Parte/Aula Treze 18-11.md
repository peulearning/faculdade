# Técnicas de Integração: Substituição vs. Integral por Partes

## Introdução
Ao resolver integrais, duas técnicas são fundamentais: a **Substituição** (regra da cadeia inversa) e a **Integral por Partes** (regra do produto inversa).

---

## 1. Integração por Substituição

### 💡 Conceito
Usada quando a integral contém uma função composta $f(g(x))$ e a derivada da função interna $g'(x)$ aparece multiplicando no integrando (ou pode ser ajustada).

### 📝 Algoritmo
1. **Escolha:** Defina $u = g(x)$ (a função "de dentro").
2. **Diferencial:** Calcule $du = g'(x) \, dx$.
3. **Substituição:** Reescreva a integral inteira em termos de $u$.
4. **Integração:** Resolva em $u$.
5. **Retorno:** Substitua $u$ de volta por $g(x)$.

### 🔎 Exemplo Prático
Calcule:
$$\int 2x \cdot e^{x^2} \, dx$$

**Solução:**
1. Escolhemos a função interna (o expoente) para ser $u$:
   $$u = x^2 \quad \Longrightarrow \quad du = 2x \, dx$$

2. Substituímos na integral original:
   $$
   \begin{aligned}
   \int e^{\underbrace{x^2}_{u}} \cdot \underbrace{2x \, dx}_{du} &= \int e^u \, du \\
   &= e^u + C
   \end{aligned}
   $$

3. Voltamos para a variável $x$:
   $$\boxed{e^{x^2} + C}$$

---

## 2. Integral por Partes

### 💡 Conceito
Baseada na regra do produto, é usada para integrar produtos de funções de naturezas diferentes (ex: polinômio $\times$ exponencial).

> [!INFO] A Fórmula
> $$\int u \, dv = u \cdot v - \int v \, du$$
> *"Um dia vi um velho menos a integral de v du"*

### 🧠 Dica: Como escolher o $u$? (Regra LIATE)
A prioridade para escolher quem será o **$u$** segue esta ordem:
1. **L**ogos (Logarítmicas: $\ln x$)
2. **I**nversas (Trigonométricas Inversas: $\arctan x$)
3. **A**lgébricas (Polinômios: $x^2, 3x$)
4. **T**rigonométricas ($\sin x, \cos x$)
5. **E**xponenciais ($e^x$)

### 🔎 Exemplo Prático
Calcule:
$$\int x \cdot e^x \, dx$$

**Solução:**
1. **Setup:** Usando LIATE, Algébrica ($x$) tem preferência sobre Exponencial ($e^x$) para ser o $u$.

$$
\begin{aligned}
\text{Derivar } (\downarrow) \quad & \quad \text{Integrar } (\uparrow) \\
u = x \quad & \quad dv = e^x \, dx \\
du = dx \quad & \quad v = e^x
\end{aligned}
$$

2. **Aplicação da Fórmula:**
   $$
   \begin{aligned}
   \int \underbrace{x}_{u} \underbrace{e^x \, dx}_{dv} &= \underbrace{x}_{u} \cdot \underbrace{e^x}_{v} - \int \underbrace{e^x}_{v} \underbrace{\, dx}_{du} \\
   &= x e^x - \int e^x \, dx \\
   &= x e^x - e^x + C
   \end{aligned}
   $$

3. **Resultado:**
   $$\boxed{e^x(x - 1) + C}$$

---

## 3. Exercícios de Fixação

### Exercício 1 (Substituição)
$$\int \frac{3x^2}{1 + x^3} \, dx$$

> 	[!success]- Ver Solução
> 	**Escolha:** $u = 1 + x^3 \Rightarrow du = 3x^2 \, dx$.
>	
> 	$$
> 	\begin{aligned}
> 	\int \frac{1}{u} \, du &= \ln|u| + C \\
> 	&= \ln|1 + x^3| + C
> 	\end{aligned}
> 	$$

### Exercício 2 (Por Partes)
$$\int x \ln(x) \, dx$$

> [!success]- Ver Solução
> **Escolha (LIATE):** Logarítmica tem prioridade.
> $$
> \begin{aligned}
> u &= \ln(x) & dv &= x \, dx \\
> du &= \frac{1}{x} \, dx & v &= \frac{x^2}{2}
> \end{aligned}
> $$
>
> **Aplicação:**
> $$
> \begin{aligned}
> \int x \ln(x) \, dx &= \frac{x^2}{2}\ln(x) - \int \frac{x^2}{2} \cdot \frac{1}{x} \, dx \\
> &= \frac{x^2}{2}\ln(x) - \frac{1}{2}\int x \, dx \\
> &= \frac{x^2}{2}\ln(x) - \frac{x^2}{4} + C
> \end{aligned}
> $$

---

## Conclusão
* Use **Substituição** para "desfazer" a Regra da Cadeia.
* Use **Por Partes** para "desfazer" a Regra do Produto.