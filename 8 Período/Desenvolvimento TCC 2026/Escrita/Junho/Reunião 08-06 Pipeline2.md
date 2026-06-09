#### 🔬 Experimentos de Pré-processamento (Novos Notebooks)

Para a próxima discussão, é necessário derivar o notebook principal em duas novas frentes de experimentação usando imagens em **escala de cinza**:

- [ ] **Notebook 1: Edge Canny**
    
    - Converter imagens para tons de cinza.
        
    - Aplicar o filtro de Canny para detecção de bordas (focando nos contornos das estruturas/lesões).
        
    - Treinar o modelo com esses dados e extrair métricas.
        
- [ ] **Notebook 2: CLAHE (Contrast Limited Adaptive Histogram Equalization)**
    
    - Converter imagens para tons de cinza.
        
    - Aplicar o CLAHE. Essa técnica é excelente para imagens clínicas, pois realça o contraste local sem estourar o brilho global, destacando melhor a textura dos tecidos.
        
    - Treinar o modelo com esses dados e extrair métricas.
        
- [ ] **Comparativo Final:** Montar um quadro comparativo dos resultados do Canny e do CLAHE contra a _baseline_ original (RGB sem filtros).
