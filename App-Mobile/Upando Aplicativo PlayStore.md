## 🔍 O Diagnóstico do Problema

O Google Play utiliza duas informações para controlar as versões:

1. **`version` (ou `versionName`):** É a versão que o usuário final vê na loja (Ex: `1.0.0`, `1.1.2`).
    
2. **`versionCode`:** É um **número inteiro** que o Google usa internamente para saber qual build é mais recente (Ex: `1`, `2`, `3`). **Este número precisa obrigatoriamente subir (+1) a cada novo upload.** Se o último upload foi o código `4`, o próximo tem que ser o `5`.
    

---

## 🛠️ Passo a Passo para Resolver

### Passo 1: Descobrir a versão atual na Play Store

Como não se sabe em qual versão o app está no Console,  precisamos descobrir o maior `versionCode` já enviado:

1. Acesse o **Google Play Console** com a conta do Bernardo. Feito ✅
    
2. Selecione o aplicativo em questão. Feito ✅
     
3. No menu lateral esquerdo, vá até a seção **Versão** e clique em **Explorador de pacotes de apps** (App Bundle Explorer).  
Resposta :  A versão e 6 (2.3.0)


4. Lá você verá uma lista de todos os arquivos `.aab` enviados. Olhe a coluna **Versão** e identifique o maior número inteiro (o `versionCode`) que está lá.
    
    > _Exemplo: Se o maior número que você encontrar lá for `14`, o seu próximo arquivo precisará ser o `15`._
    


### Passo 2: Atualizar o código local no Expo

Agora que vocês sabem qual é o último número da Play Store, é hora de ajustar o projeto local.

1. No VS Code (ou editor de código), abra o arquivo **`app.json`** na raiz do projeto Expo.
    
2. Procure pela chave `"android"`. Você precisará ajustar duas linhas:

```
{
  "expo": {
    "name": "Nome do App",
    "slug": "nome-do-app",
    "version": "1.0.1", // 👈 Altere aqui a versão visual se quiser (opcional)
    ...
    "android": {
      "package": "com.bernardo.app",
      "versionCode": 15, // 👈 AQUI ESTÁ O SEGREDO! Esse número DEVE ser maior do que o maior número que você viu na Play Store.
      ...
    }
  }
}
```


### Passo 3: Gerar o novo `.aab` e fazer o Upload

Com o `app.json` salvo e com o `versionCode` corrigido:

1. Gere o novo arquivo `.aab`. Se ele usa o EAS Build, o comando padrão é:

```
eas build --platform android
```

- _(Caso ele use as credenciais locais e faça o build localmente, certifique-se de que a keystore e a senha que você tem em mãos sejam aplicadas corretamente no build)._
    
- Pegue o novo arquivo `.aab` gerado.
    
- Volte ao Google Play Console, vá na aba de **Teste Interno**, clique em **Criar novo lançamento** (ou editar lançamento atual) e arraste o novo arquivo para lá.

## ⚠️ Cuidados com a Keystore (Chave de Assinatura)

Como você mencionou que tem a chave e a senha:

- Se vocês estiverem usando o **EAS Build (Expo)**, o próprio servidor do Expo costuma gerenciar isso se configurado na primeira vez.
    
- Se estiverem gerando o build localmente via Android Studio ou CLI (sem EAS), certifiquem-se de que as credenciais no arquivo `eas.json` ou nas variáveis de ambiente apontem exatamente para o arquivo `.keystore` e usem a senha correta que você possui. Se assinar com uma chave diferente da que está na Play Store, o Google também rejeitará o arquivo.

* O Bernardo costuma gerar esse `.aab` usando o EAS Build da própria Expo ou ele faz o processo de build de forma local na máquina dele?

--- 

## 🛠️ Parte de Gerar .aab e Setar Configs


1. npx expo prebuild --clean # gera uma nova pasta android, com as alterações
    
2. deixo o .keystore de backup na raiz, e também salvo em um disco de backup
    
3. copio o arquivo .keystore da raiz para a pasta android > app
    
4. E altero dentro de build.gradle
    
5. signingConfigs { release { if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) { storeFile file(MYAPP_UPLOAD_STORE_FILE) storePassword MYAPP_UPLOAD_STORE_PASSWORD keyAlias MYAPP_UPLOAD_KEY_ALIAS keyPassword MYAPP_UPLOAD_KEY_PASSWORD } } } buildTypes { release { ... signingConfig signingConfigs.release } }
    
6. E dentro de android > gradle.properties
    
7. MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore MYAPP_UPLOAD_KEY_ALIAS=my-key-alias MYAPP_UPLOAD_STORE_PASSWORD=your_store_password MYAPP_UPLOAD_KEY_PASSWORD=your_key_password
    
8. ./gradlew bundleRelease
    
9. arquivo gerado na pasta android > app > builds > outputs > bundle > release
    
10. e envio para a play store

