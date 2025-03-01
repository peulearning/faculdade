1. **Problemas Abordados**:

- **Escassez de dermatologistas**: Especialmente em regiões rurais, dificultando o acesso a diagnósticos precisos.
- **Interpretação de Imagens**: Desafios na avaliação precisa de imagens de doenças de pele.
- **Relatórios Diagnósticos**: A geração de relatórios amigáveis para os pacientes é demorada.

1. **Tecnologias Utilizadas**:

- **SkinGPT-4**: Sistema de diagnóstico dermatológico interativo baseado em um modelo de linguagem visual avançado (MiniGPT-4).
- **MiniGPT-4**: Incorporado para alavancar informações visuais com modelos de linguagem, utilizando a arquitetura Vicuna como decodificador de linguagem.
- **Vision Transformer (ViT)**: Para processamento e compreensão de imagens de doenças de pele.
- **Q-Former**: Usado em conjunto com o ViT para gerar embeddings e compreender o contexto das imagens.

1. **Datasets**:

- **Treinamento**: SkinGPT-4 foi treinado em um extenso conjunto de dados de imagens de doença de pele, com **52.929 imagens**, combinando dados públicos e proprietários.
- **Step 1 Dataset**: Inclui imagens com conceitos clínicos como eritema, placas, e pápulas, totalizando **3.886 amostras**.
- **Step 2 Dataset**: Contém categorias de doenças da pele com amostras específicas (ex: Acne e Rosácea - 840, Melanomas - 23.373).
- **Avaliação**: **150 casos reais** avaliados por dermatologistas certificados para validar a precisão dos diagnósticos.

1. **Limitações**:

- **Não Substituto para Médicos**: Embora útil, SkinGPT-4 não deve substituir a consulta com profissionais de saúde.
- **Dependência de Dados de Treinamento**: O desempenho é fortemente influenciado pela qualidade e diversidade dos dados usados para treinamento.
- **Desafios de Interpretação**: Diagnósticos complexos podem ainda ser problemáticos, exigindo validação humana em muitos cenários.

1. **Melhorias e Inovações**:

- **Treinamento em Duas Etapas**: Alinhamento visual e textual em duas etapas melhora a capacidade do modelo de expressar características clínicas em linguagem natural.
- **Interatividade**: Permite que os usuários carreguem fotos para diagnósticos, promovendo um entendimento mais claro das condições de saúde.
- **Privacidade do Usuário**: A capacidade de ser implantado localmente ajuda a garantir a segurança dos dados dos pacientes.

1. **Padrões e Avaliações**:

- **Estratégias de Avaliação**: Dermatologistas revisaram e avaliaram casos com critérios como precisão diagnóstica, clareza de descrição e utilidade das recomendações.
- **Resultados Quantitativos**: A pesquisa demonstrou que SkinGPT-4 oferece diagnósticos precisos e melhora na comunicação entre pacientes e médicos.

Github : https://github.com/JoshuaChou2018/SkinGPT-4