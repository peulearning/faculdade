
```


import os
import shutil
from sklearn.model_selection import train_test_split

dataset_master = "/content/drive/MyDrive/dataset_master_output" 
dataset_split = "/content/drive/MyDrive/dataset_master_split"

# Remove split anterior
if os.path.exists(dataset_split):
    shutil.rmtree(dataset_split)

# Cria estrutura (Removido "validation")
for subset in ["train", "test"]:
    os.makedirs(os.path.join(dataset_split, subset), exist_ok=True)

# Processa cada classe separadamente
for classe in os.listdir(dataset_master):

    classe_path = os.path.join(dataset_master, classe)

    if not os.path.isdir(classe_path):
        continue

    imagens = [
        f for f in os.listdir(classe_path)
        if f.lower().endswith((".jpg", ".jpeg", ".png"))
    ]

    # Divisão Direta: Treino e Teste
    # Altere o test_size aqui para 0.30, 0.20 ou 0.25 conforme o teste
    train_imgs, test_imgs = train_test_split(
        imagens,
        test_size=0.30, 
        random_state=42,
        shuffle=True
    )

    # cria pastas (Removido "validation")
    for subset in ["train", "test"]:
        os.makedirs(
            os.path.join(dataset_split, subset, classe),
            exist_ok=True
        )

    # copiar treino
    for img in train_imgs:
        shutil.copy2(
            os.path.join(classe_path, img),
            os.path.join(dataset_split, "train", classe, img)
        )

    # copiar teste (Caminho corrigido para evitar o erro do "if False")
    for img in test_imgs:
        shutil.copy2(
            os.path.join(classe_path, img),
            os.path.join(dataset_split, "test", classe, img)
        )

print("Split criado com sucesso!")
```


