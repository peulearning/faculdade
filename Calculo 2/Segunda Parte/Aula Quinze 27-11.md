:

# 🧮 Integrais Trigonométricas: Estratégias Fundamentais

Nesta seção, focamos nas estratégias para resolver **Integrais de Potências do Seno e Cosseno** $(\int \text{sen}^m x \cdot \text{cos}^n x \ dx)$.

---

## 1. 🥇 Estratégia para Potências Ímpares (Pelo menos um expoente ímpar)

A chave é **isolar um fator $\text{sen} \ x$ ou $\text{cos} \ x$** (da função com o expoente ímpar) e usar a **Identidade Pitagórica** para converter o restante da função na cofunção. Isso prepara a integral para uma simples substituição $u$.

### 📚 Identidade Pitagórica

- $\text{sen}^2 x = 1 - \text{cos}^2 x$
    
- $\text{cos}^2 x = 1 - \text{sen}^2 x$
    

|**Condição**|**Estratégia**|**Substituição Recomendada**|
|---|---|---|
|**$n$** é **ímpar** ($\text{cos}^n x$)|Isole $\text{cos} \ x$ e use $\text{cos}^2 x = 1 - \text{sen}^2 x$|$u = \text{sen} \ x$|
|**$m$** é **ímpar** ($\text{sen}^m x$)|Isole $\text{sen} \ x$ e use $\text{sen}^2 x = 1 - \text{cos}^2 x$|$u = \text{cos} \ x$|

---

## 2. 🥈 Estratégia para Potências Pares (Ambos $m$ e $n$ são pares)

Quando ambos os expoentes são pares, devemos usar as **Identidades do Ângulo-Metade** para reduzir a potência (e dobrar o argumento do ângulo). Isso é repetido até que a integral possa ser resolvida.

### 📐 Identidades do Ângulo-Metade

- $$\text{sen}^2 x = \frac{1}{2}(1 - \text{cos} \ 2x)$$
    
- $$\text{cos}^2 x = \frac{1}{2}(1 + \text{cos} \ 2x)$$
    
- $$\text{sen} \ x \cdot \text{cos} \ x = \frac{1}{2} \text{sen} \ 2x$$
    
    (Essa é útil quando a integral pode ser reescrita na forma $(\text{sen} \ x \cdot \text{cos} \ x)^k$)
    

---

## 💡 Exercícios Práticos

Aplique as estratégias nas seguintes integrais:

1. $$\int \text{sen}^5 x \cdot \text{cos}^2 x \ dx$$
    
2. $$\int \text{cos}^3 x \ dx$$
    
3. $$\int_{0}^{\pi} \text{sen}^2 x \ dx$$
    
4. $$\int \text{sen}^3 x \cdot \text{cos}^3 x \ dx$$
    

---

## 📚 Notas Adicionais

- Lembre-se: A estratégia para **potências ímpares** é geralmente mais direta, pois envolve apenas uma substituição $u$.
    
- A estratégia para **potências pares** costuma ser mais longa, pois exige a aplicação das identidades de redução de potência.