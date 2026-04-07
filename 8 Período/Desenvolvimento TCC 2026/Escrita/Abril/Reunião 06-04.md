
# 📅 Reunião TCC - Suzana (06/04/2026)

## 📌 Tópicos Discutidos
* **Imagens Parecidas:** Necessidade de avaliar a similaridade entre as imagens do dataset.
* **Data Augmentation:** * As técnicas de augmentation estão impactando negativamente **2 classes específicas (D, P)**, causando mais confusão do que ajudando o modelo a generalizar.
  * Ferramenta de apoio sugerida: [Albumentations Explore](https://explore.albumentations.ai).
* **Perfis das Imagens:** Foram identificados diferentes perfis visuais:
  * Vermelhas
  * Amareladas
  * Necrosadas
  * Variações de iluminação: Algumas mais claras, outras mais escuras.
* ⚠️ **Atenção (Desbalanceamento):** Existe uma quantidade muito pequena de imagens do perfil *Necrosadas*.

---

## 🔬 Hipótese Principal
> Será que os erros de predição que o modelo está cometendo estão concentrados em uma dessas subclasses de perfis visuais específicos (ex: apenas nas amareladas ou apenas nas mais escuras)?

---

## ✅ Checklist de Próximos Passos

- [ ] Analisar os resultados das imagens antes e depois do *Data Augmentation* (usar o [Albumentations Explore](https://explore.albumentations.ai) como referência).
- [ ] Investigar a fundo o motivo do *Augmentation* estar confundindo as classes **D** e **P**. (Talvez testar outras transformações ou remover as atuais para essas classes).
- [ ] Validar a hipótese: mapear se os erros do modelo se concentram em imagens vermelhas, amareladas, necrosadas, claras ou escuras.
- [ ] Implementar script para exibir lado a lado a imagem, o que foi predito e o rótulo real (análise a olho nu), com foco especial no cenário de 2 Classes.
- [ ] Avaliar uma estratégia para lidar com a escassez de imagens da classe/perfil *Necrosadas* (ex: coleta de novos dados, pesos nas classes, oversampling).

---

## 💻 Códigos Úteis

### Avaliação detalhada com matriz de confusão
Script para plotar a matriz de confusão e o relatório de classificação do modelo atual:

```python

from sklearn.metrics import classification_report, confusion_matrix, ConfusionMatrixDisplay
import matplotlib.pyplot as plt
import numpy as np

# ==========================================
# 1. PREDIÇÕES E AVALIAÇÃO GLOBAL
# ==========================================
y_true = test_generator.classes
y_pred_probs = modelo.predict(test_generator)
y_pred = np.argmax(y_pred_probs, axis=1)
class_names = list(test_generator.class_indices.keys())

# Matriz de Confusão
cm = confusion_matrix(y_true, y_pred)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=class_names)
disp.plot(cmap=plt.cm.Blues)
plt.title("Matriz de Confusão - Desempenho Geral")
plt.xticks(rotation=45)
plt.savefig("matriz_confusao.png", bbox_inches='tight') # Salva a imagem para o TCC
plt.show()

# Classification Report
report = classification_report(y_true, y_pred, target_names=class_names)
print("\n📊 Relatório de Classificação:\n")
print(report)

# ==========================================
# 2. ANÁLISE A OLHO NU (Filtro de Erros)
# ==========================================
print("\n👁️ Extraindo imagens classificadas incorretamente para validação da hipótese...\n")

# Encontrar os índices onde o modelo errou
erros_indices = np.where(y_pred != y_true)[0]

if len(erros_indices) == 0:
    print("O modelo acertou todas as predições no conjunto de teste!")
else:
    # Pegar até 10 erros para visualizar
    num_erros_mostrar = min(10, len(erros_indices))
    indices_para_mostrar = erros_indices[:num_erros_mostrar]
    
    plt.figure(figsize=(15, 6))
    plt.suptitle("Análise de Erros: Real vs Predito (Avaliando Perfis de Cor/Necrose)", fontsize=16, y=1.05)
    
    # Como test_generator pode não dar acesso direto à imagem por índice facilmente,
    # vamos buscar os caminhos dos arquivos de teste que falharam
    filepaths = test_generator.filepaths
    
    for i, idx in enumerate(indices_para_mostrar):
        plt.subplot(2, 5, i + 1)
        
        # Carrega a imagem original que o modelo errou
        img_path = filepaths[idx]
        img = plt.imread(img_path)
        plt.imshow(img)
        
        true_label = class_names[y_true[idx]]
        pred_label = class_names[y_pred[idx]]
        
        plt.title(f"Real: {true_label}\nPred: {pred_label}", color="red", fontweight='bold')
        plt.axis("off")

    plt.tight_layout()
    plt.savefig("analise_erros_visuais.png", bbox_inches='tight') # Salva a imagem para o TCC
    plt.show()
    ```
``` 

---

