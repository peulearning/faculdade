# 📚 Integração por Partes e Substituição — Notas de Cálculo

## 🧠 Resumo da Aula e Próximos Passos
* **Conteúdo Principal:** Revisão completa de **Integração por Partes** ($\int u,dv$) e reforço da importância da **Substituição** ($\int f(g(x))g'(x),dx$).
* **Exemplo-Chave:** O método de solução por sistema linear para a integral recorrente $\int e^{x}\sin x,dx$.
* **Próxima Aula:** Iniciaremos as integrações envolvendo **funções trigonométricas** (partes ou substituições).

---

## 1. 🔗 A Regra da Integração por Partes

A Integração por Partes (IP) é a versão integral da **Regra do Produto da Derivada**.

### 📝 A Fórmula
* **Partindo da Regra do Produto:**
    $$\frac{d}{dx}\big(u(x)v(x)\big) = u'(x)v(x) + u(x)v'(x).$$
* **Integrando e Reorganizando (Fórmula da IP):**
    $$\boxed{\int u,dv = uv - \int v,du}$$

### ⚙️ Como Usar (Passos-Chave)
1.  **Escolha** $\mathbf{u}$ (a parte a derivar) e $\mathbf{dv}$ (a parte a integrar).
2.  Calcule $\mathbf{du = u',dx}$ e $\mathbf{v = \int dv}$.
3.  Substitua na fórmula e resolva a integral $\int v,du$.

> [!DICA] Regra Mnemônica LIPET para Escolher $\mathbf{u}$
> **L** — **L**ogarítmicas ($\ln x$)
> **I** — **I**nversas Trigonométricas ($\arctan x$)
> **P** — **P**olinomiais ($x^n$)
> **E** — **E**xponenciais ($e^x$)
> **T** — **T**rigonométricas ($\sin x, \cos x$)
>
> 💡 Escolha como **$\mathbf{u}$ a função que aparece mais à esquerda** na lista. O resto será $\mathbf{dv}$.

---

## 2. 🧮 Integração Definida por Partes

Ao aplicar a IP a uma integral definida $\int_a^b u,dv$:
$$\int_a^b u,dv = \Big[uv\Big]_a^b - \int_a^b v,du$$

> [!ALERTA] Atenção com os Limites
> 1.  O termo $\mathbf{uv}$ (o da frente) **deve ser avaliado** nos limites de integração: $\Big[uv\Big]_a^b = u(b)v(b) - u(a)v(a)$.
> 2.  A integral $\mathbf{\int v,du}$ (o termo de trás) é uma integral definida **normal** e deve ser resolvida com seus limites originais $\mathbf{a}$ e $\mathbf{b}$.

### Exemplo Definido: $\int_{0}^{\pi} x\sin x,dx$
* $u = x \implies du = dx$
* $dv = \sin x,dx \implies v = -\cos x$
$$\int_{0}^{\pi} x\sin x,dx = \Big[ -x\cos x \Big]_{0}^{\pi} + \int_{0}^{\pi} \cos x,dx$$
* Primeiro Termo: $\Big[-x\cos x\Big]_0^{\pi} = (-\pi\cos\pi) - (-0\cos 0) = \pi$.
* Segundo Termo: $\int_{0}^{\pi} \cos x,dx = \big[\sin x\big]_0^{\pi} = 0 - 0 = 0$.
$$\boxed{\int_{0}^{\pi} x\sin x,dx = \pi}$$

---

## 3. 🔄 Substituição vs. Partes (e Combinação)

A **Substituição** é essencial para integrar composições de funções.

### Fórmula da Substituição
Se $t=g(x)$ (e $dt=g'(x)dx$):
$$\int f(g(x))g'(x),dx = \int f(t),dt$$

### 🛠️ Estratégia de Combinação
* **Substituição Antes de Partes:** Use a substituição para **simplificar** uma integral antes de aplicar IP.
* **Substituição Após Partes:** A integral $\int v,du$ gerada pela IP exige uma substituição para ser resolvida.

---

## 4. 💫 Exemplo Recorrente: $\int e^{x}\sin x,dx$

Integrais com $\mathbf{e^x}$ e funções trigonométricas geram um **sistema linear**.

1.  **Primeira Parte ($\mathbf{I}$):** $I = e^{x}\sin x - \int e^{x}\cos x,dx \implies I = e^{x}\sin x - J$
2.  **Segunda Parte ($\mathbf{J}$):** $J = e^{x}\cos x + \underbrace{\int e^{x}\sin x,dx}_{I} \implies J = e^{x}\cos x + I$
3.  **Resolução do Sistema:**
    $$I = e^{x}\sin x - (e^{x}\cos x + I)$$
    $$2I = e^{x}(\sin x - \cos x)$$
    $$\boxed{\int e^{x}\sin x,dx = \frac{e^{x}}{2}(\sin x - \cos x) + C}$$

---

## 5. ⚠️ Estratégias e Armadilhas Comuns
* **Erro na Base:** O erro mais comum é **integrar $dv$ incorretamente** ao calcular $v$.
* **Integrais de "1":** Para $\int \ln x,dx$ ou $\int \arctan x,dx$, defina $dv = 1,dx$.
* **Isolamento:** Se após a IP a integral $\int v,du$ for igual à integral original $I$, **isole $I$** (somando-a aos dois lados da equação).

---

## 6. 📝 Resoluções Detalhadas (Exercícios)

### Ex. 1: $\int x e^{x},dx$
* $u = x$, $dv = e^x dx$
$$\int x e^{x},dx = x e^{x} - \int e^{x},dx = \boxed{e^{x}(x-1) + C}$$

### Ex. 2: $\int \ln x,dx$
* $u = \ln x$, $dv = dx$
$$\int \ln x,dx = x\ln x - \int x\cdot \frac{1}{x},dx = x\ln x - \int 1,dx = \boxed{x\ln x - x + C}$$

### Ex. 3: $\int x\cos x,dx$
* $u = x$, $dv = \cos x,dx$
$$\int x\cos x,dx = x\sin x - \int \sin x,dx = \boxed{x\sin x + \cos x + C}$$

### Ex. 4: $\int_{0}^{1} x\ln x,dx$
* $u = \ln x$, $dv = x,dx$
$$\int_{0}^{1} x\ln x,dx = \Big[\frac{x^{2}}{2}\ln x\Big]{0}^{1} - \int_{0}^{1} \frac{x^{2}}{2}\cdot \frac{1}{x},dx$$
* Primeiro Termo: $\Big[\frac{x^{2}}{2}\ln x\Big]{0}^{1} = 0 - \lim_{x\to 0^{+}} \frac{x^{2}}{2}\ln x = 0-0=0$.
* Segundo Termo: $-\frac{1}{2}\int_{0}^{1} x,dx = -\frac{1}{2}\cdot \frac{x^{2}}{2}\Big|_{0}^{1} = -\frac{1}{4}$.
$$\boxed{\int_{0}^{1} x\ln x,dx = -\frac{1}{4}}$$

### Ex. 6: $\int x^{2}\ln x,dx$
* $u = \ln x$, $dv = x^{2}dx$
$$\int x^{2}\ln x,dx = \frac{x^{3}}{3}\ln x - \int \frac{x^{3}}{3}\cdot \frac{1}{x},dx = \frac{x^{3}}{3}\ln x - \frac{1}{3}\int x^{2},dx$$
$$\int x^{2}\ln x,dx = \frac{x^{3}}{3}\ln x - \frac{x^{3}}{9} + C = \boxed{\frac{x^{3}}{3}\left(\ln x - \frac{1}{3}\right) + C}$$