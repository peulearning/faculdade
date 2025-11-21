# Técnicas de Integração: Substituição vs. Integral por Partes

## Introdução
Ao resolver integrais, duas técnicas são frequentemente utilizadas: a *substituição* e a *integral por partes*. Saber quando aplicar cada uma pode facilitar o processo de integração e levar a uma solução mais simples.

---

## 1. Integração por Substituição

### Definição
A técnica de substituição é usada quando a integral contém uma função composta. Nessa técnica, substituímos uma parte da integral por uma nova variável, simplificando o processo.

### Quando Usar
- Quando a integral contém uma função dentro de outra função (ex: \( f(g(x)) \)).
- Quando podemos derivar a função interna, tornando a substituição viável.

### Passos
1. **Escolha a Substituição**: Defina \( u = g(x) \).
2. **Calcule \( du \)**: Derive \( u \) para encontrar \( du = g'(x)dx \).
3. **Substitua**: Troque \( g(x) \) e \( dx \) na integral original.
4. **Integre**: Resolva a nova integral em termos de \( u \).
5. **Substitua de Volta**: Converta para a variável original \( x \).

### Exemplo
Calcule:
\[
\int 2x \cdot e^{x^2} \, dx
\]

**Solução**:
1. **Escolha \( u = x^2 \)**, então \( du = 2x \, dx \).
2. A integral se transforma em:
   \[
   \int e^u \, du = e^u + C = e^{x^2} + C
   \]

---

## 2. Integral por Partes

### Definição
A integral por partes é baseada na fórmula \( \int u \, dv = uv - \int v \, du \) e é útil quando a integral é um produto de duas funções.

### Quando Usar
- Quando a integral envolve o produto de funções que podem ser derivadas (ex: \( x \cdot \sin(x) \)).
- Quando uma das funções pode ser facilmente integrada.

### Passos
1. **Escolha \( u \) e \( dv \)**: Identifique uma parte da integral como \( u \) (frequentemente a parte que se torna mais simples quando derivada) e a outra como \( dv \).
2. **Derive \( u \)**: Encontre \( du \).
3. **Integre \( dv \)**: Encontre \( v \).
4. **Aplique a Fórmula**: Utilize \( \int u \, dv = uv - \int v \, du \).
5. **Resolva**: Simplifique e integre a nova integral.

### Exemplo
Calcule:
\[
\int x \cdot e^x \, dx
\]

**Solução**:
1. **Escolha \( u = x \)** e \( dv = e^x \, dx \).
2. **Derive**: \( du = dx \); **Integre**: \( v = e^x \).
3. Aplique a fórmula:
   \[
   \int x \cdot e^x \, dx = x \cdot e^x - \int e^x \, dx = x \cdot e^x - e^x + C
   \]

---

## Exercícios

### Exercício 1: Substituição
Calcule a integral:
\[
\int \frac{3x^2}{1 + x^3} \, dx
\]

### Exercício 2: Integral por Partes
Calcule a integral:
\[
\int x \ln(x) \, dx
\]

### Respostas
1. **Exercício 1**: A solução envolve fazer a substituição \( u = 1 + x^3 \).
2. **Exercício 2**: Use \( u = \ln(x) \) e \( dv = x \, dx \) na integral por partes.

---

## Conclusão
A escolha entre substituição e integral por partes depende da forma da função integral. Pratique com exercícios variados para dominar essas técnicas e saber aplicá-las de forma eficaz em diferentes situações.   ✅