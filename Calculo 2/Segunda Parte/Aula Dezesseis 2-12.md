---
tags:
  - calculo2
  - integrais
  - prova2
data: 2025-12-02
---

# 📚 Integrais Trigonométricas (Estratégias)

> [!DANGER] 📅 Próxima Avaliação (P2)
> **Data:** 18/12
> **Escopo:** Todo o conteúdo visto até a aula de hoje.

---

## 1. Estratégias para $\int \tan^m(x) \sec^n(x) \, dx$

A abordagem depende da potência da tangente ou da secante.

| Caso | Condição | Estratégia (Passo a Passo) | Substituição |
| :--- | :--- | :--- | :--- |
| **1** | **Secante Par** ($n$ par) | 1. Separe um fator $\sec^2(x)$.<br>2. Converta o resto usando $\sec^2(x) = 1 + \tan^2(x)$. | $u = \tan(x)$ |
| **2** | **Tangente Ímpar** ($m$ ímpar) | 1. Separe um fator $\sec(x)\tan(x)$.<br>2. Converta o resto usando $\tan^2(x) = \sec^2(x) - 1$. | $u = \sec(x)$ |

### 💡 Resultados Úteis (Memorizar)

$$
\int \tan(x) \, dx = \ln |\sec(x)| + C
$$

$$
\int \sec(x) \, dx = \ln |\sec(x) + \tan(x)| + C
$$

---

## 2. Transformação de Produto em Soma
Utilizadas para resolver integrais com produtos de seno e cosseno com argumentos diferentes (ex: $\sin(4x)\cos(5x)$).

> [!INFO] Fórmulas de Prostaférese
> **A) Seno $\cdot$ Cosseno:**
> $$\sin A \cdot \cos B = \frac{1}{2} [\sin(A-B) + \sin(A+B)]$$
> 
> **B) Seno $\cdot$ Seno:**
> $$\sin A \cdot \sin B = \frac{1}{2} [\cos(A-B) - \cos(A+B)]$$
> 
> **C) Cosseno $\cdot$ Cosseno:**
> $$\cos A \cdot \cos B = \frac{1}{2} [\cos(A-B) + \cos(A+B)]$$

---

## 3. 📝 Exemplos de Aula

Rever a resolução destes exercícios para a prova:

- [ ] **Ex 1:** $\displaystyle \int \tan^6(x) \cdot \sec^4(x) \, dx$ *(Caso da Secante Par)*
- [ ] **Ex 2:** $\displaystyle \int \tan^5(x) \cdot \sec^7(x) \, dx$ *(Caso da Tangente Ímpar)*
- [ ] **Ex 3:** $\displaystyle \int \tan^3(x) \, dx$
- [ ] **Ex 4:** $\displaystyle \int \sec^3(x) \, dx$ *(Integração por Partes / Recorrente)*
- [ ] **Ex 5:** $\displaystyle \int \sin(4x)\cos(5x) \, dx$ *(Uso da fórmula A)*