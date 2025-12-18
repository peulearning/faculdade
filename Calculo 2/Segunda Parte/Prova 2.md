# 📝 Resolução da Prova de Cálculo: Técnicas de Integrais

---

## 1. Verdadeiro ou Falso (Justificativas)

> [!check] 1.a) $\int \tan^{-1}(x) \, dx$ por partes?
> **(Verdadeiro)**
> Podemos usar $u = \arctan(x)$ e $dv = dx$. O resultado gera uma integral algébrica simples.

> [!check] 1.b) $\int x^3 e^x \, dx$ requer partes 3 vezes?
> **(Verdadeiro)**
> Para reduzir o polinômio $x^3$ a uma constante, precisamos derivá-lo 3 vezes sucessivas através do método de integração por partes.

> [!fail] 1.c) $\int \tan^3 \sec x \, dx$: usar $\tan^2 = \sec^2 - 1$ e guardar $\tan x$?
> **(Falso)**
> A estratégia correta para potências ímpares de tangente é guardar o fator **$\sec x \tan x$** (a derivada da secante) para compor o $du$.
> *Integral correta:* $\int (\sec^2 x - 1) \cdot (\sec x \tan x) \, dx$.

---

## 2. Cálculo de $\int \sin x \cos x \, dx$

> [!abstract] Diferentes Métodos
>
> **2.a) Substituição $u = \sin x$**
> $$\frac{\sin^2 x}{2} + C$$
>
> **2.b) Substituição $u = \cos x$**
> $$-\frac{\cos^2 x}{2} + C$$
>
> **2.c) Identidade $\sin(2x) = 2\sin x \cos x$**
> $$-\frac{\cos(2x)}{4} + C$$
>
> **2.d) Por Partes**
> $$\frac{\sin^2 x}{2} + C$$

> [!info] Conclusão (2.e)
> Todos os métodos estão corretos. As respostas diferem apenas pela constante de integração, pois $\sin^2 x$ e $\cos^2 x$ estão relacionados pela identidade fundamental, e $\cos(2x)$ pelas identidades de arco duplo.

---

## 3. Cálculo das Integrais

### 3.a) Integral Definida
$$\int_1^2 x^3 \ln x \, dx$$
*Método:* Integração por Partes ($u=\ln x, dv=x^3 dx$).
**Resultado:**
$$4\ln 2 - \frac{15}{16}$$

### 3.b) Potências Trigonométricas
$$\int \sin^4 x \cos^3 x \, dx$$
*Método:* Separar $\cos x$ e converter $\cos^2 x \to (1-\sin^2 x)$.
**Resultado:**
$$\frac{\sin^5 x}{5} - \frac{\sin^7 x}{7} + C$$

### 3.c) Produto de Cossenos
$$\int \cos(5x) \cos(2x) \, dx$$
*Método:* Prostaférese $\cos A \cos B = \frac{1}{2}[\cos(A-B) + \cos(A+B)]$.
**Resultado:**
$$\frac{1}{6}\sin(3x) + \frac{1}{14}\sin(7x) + C$$

---

## 4. Questão Extra
**Problema:** Calcular $\int x^5 \cos(x^3) \, dx$ combinando Substituição e Partes.

1. **Substituição:** $w = x^3 \implies dw = 3x^2 dx$.
   A integral vira: $\frac{1}{3} \int w \cos w \, dw$.
2. **Por Partes:** $u=w, dv=\cos w \, dw$.
   Resultado em $w$: $\frac{1}{3}(w \sin w + \cos w)$.
3. **Resultado Final:**
   $$\frac{1}{3}x^3 \sin(x^3) + \frac{1}{3}\cos(x^3) + C$$