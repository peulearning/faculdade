# 📐 Cálculo II: Aplicações de Integrais (Deve Haver Construção dos gráficos)

> [!abstract] Resumo do Conteúdo
> 
> - **Áreas:** $\int_{a}^{b} [f(x) - g(x)] \, dx$
>     
> - **Volumes (Discos):** $V = \pi \int_{a}^{b} [R(x)^2 - r(x)^2] \, dx$
>     
> - **Volumes (Cascas):** $V = 2\pi \int_{a}^{b} [raio \cdot altura] \, dx$
>     

---

## 🟦 1. Área entre Curvas (Parábolas)

### **1.a) Interseção e Região**

Dadas as curvas:

- $y = x^2 - 2x$
    
- $y = 6x - x^2$
    

**Encontrando as interseções:**

$$x^2 - 2x = 6x - x^2 \implies 2x^2 - 8x = 0 \implies 2x(x - 4) = 0$$

📍 **Pontos:** $x = 0$ e $x = 4$.

> [!info] Análise da Região
> 
> No intervalo $[0, 4]$, a curva **superior** é $y = 6x - x^2$ e a **inferior** é $y = x^2 - 2x$.

### **1.b) Cálculo da Área**

$$A = \int_{0}^{4} [(6x - x^2) - (x^2 - 2x)] \, dx$$

$$A = \int_{0}^{4} (8x - 2x^2) \, dx = \left[ 4x^2 - \frac{2}{3}x^3 \right]_{0}^{4}$$

> [!success] Resultado Final
> 
> **Área** $= \frac{64}{3} \, u.a.$

---

## 🟦 2. Volumes de Sólidos de Revolução

Região delimitada por $y = \frac{1}{4}$ e $y = 4$.

### **2.a) Rotação no Eixo $y$ (Discos)**

- **Raio Externo ($R$):** $4$
    
- **Raio Interno ($r$):** $1/4$
    
    $$V = \pi \int_{1/4}^{4} (4^2 - (1/4)^2) \, dy = \pi \int_{1/4}^{4} \frac{255}{16} \, dy$$
    

### **2.b) Rotação no Eixo $x$ (Cascas)**

- **Raio:** $y$
    
- **Altura:** $4 - \frac{1}{4} = \frac{15}{4}$
    
    $$V = 2\pi \int_{1/4}^{4} y \cdot \frac{15}{4} \, dy$$
    

> [!check] Conclusão
> 
> O volume resultante em ambos os métodos é: **$V = \frac{3825\pi}{64} \, u.v.$**

---

## 🟦 3. Funções Não Lineares e Eixo Deslocado

### **3.a) Interseções**

Curvas: $y = \sqrt{x}$ e $y = \frac{x}{2}$.

$$\sqrt{x} = \frac{x}{2} \implies x = \frac{x^2}{4} \implies x(x-4) = 0$$

📍 **Pontos:** $x = 0$ e $x = 4$.

### **3.b) Volume em relação à reta $y = 4$**

Como o eixo de rotação está acima da região:

- **Raio Externo ($R$):** $4 - \frac{x}{2}$ (distância da reta mais longe)
    
- **Raio Interno ($r$):** $4 - \sqrt{x}$ (distância da reta mais perto)
    

$$V = \pi \int_{0}^{4} \left[ \left( 4 - \frac{x}{2} \right)^2 - (4 - \sqrt{x})^2 \right] \, dx$$

---

## 🟦 Questão Extra: Área com Múltiplas Curvas

**Fronteiras:** $y = \sqrt{x-1}$, $y = 3 - x$ e $y = 0$.

> [!tip] Estratégia de Integração
> 
> Esta região exige a divisão em duas integrais no eixo $x$:
> 
> 1. **Intervalo $[1, 2]$:** Sob a curva $y = \sqrt{x-1}$
>     
> 2. **Intervalo $[2, 3]$:** Sob a reta $y = 3 - x$
>     

**Montagem:**

$$A = \int_{1}^{2} \sqrt{x-1} \, dx + \int_{2}^{3} (3-x) \, dx$$

$$A = \frac{2}{3} + \frac{1}{2}$$

> [!important] Resultado
> 
> **Área Total** $= \frac{7}{6} \, u.a.$