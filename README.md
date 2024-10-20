[Turborepo skeleton](git@github.com:logicinfocursos/turborepo_skeleton.git)

Aqui está um skeleton com um pequeno tutorial step by step para criar um monorepo com o **Turborepo**, contendo 4 subprojetos:
- api em node js com express
- adm em react js
- mobile em react native
- web em next js

# Índice
- [1. Instalação do Turborepo e Inicialização do Projeto](#instalacao)
- [2. Estrutura do Projeto](#estrutura)
- [3. Configuração do Turborepo](#configuração)
- [4. Criação do Subprojeto API (Node.js + Express)](#api)
- [5. Criação do Subprojeto ADM (React.js)](#adm)
- [6. Criação do Subprojeto Mobile (React Native)](#mobile)
- [7. Criação do Subprojeto Web (Next.js)](#next)
- [8. Pasta Compartilhada (Configurations)alação](#packages)
- [9. Configurar Importação Compartilhada](#shared)
- [10.Configuração dos Scripts no Turborepo](#config)
- [11.Configurar o turbo.json](#turbo)
- [12.Rodando o Projeto](#run)
- [13.Possíveis problemas](#problems1)
- [14.Como contornar / evitar alguns problemas já conhecidos](#problems2)
- 
Anexos
- [Fontes](#sources)
- [turborepo templates](#templates)
  
[def]: #instalacao
### 1. **Instalação do Turborepo e Inicialização do Projeto**

Primeiro, instale o Turborepo globalmente (caso ainda não tenha instalado):

```jsx
npm install -g turbo
```

Depois, crie uma pasta para o seu monorepo:

```jsx
mkdir meu-monorepo
cd meu-monorepo

```

Inicialize o projeto com o `package.json` principal:

```jsx
npm init -y
```

Agora, instale o Turborepo como dependência:

```jsx
npm install turbo --save-dev
```
[def]: #estrutura
### 2. **Estrutura do Projeto**

Crie a seguinte estrutura de pastas dentro do diretório do monorepo:

```jsx
meu-monorepo/
│
├── apps/
│   ├── api/
│   ├── adm/
│   ├── mobile/
│   └── web/
│
└── packages/
    └── configurations/
```

Aqui:

- **apps/**: onde ficarão os subprojetos.
- **packages/configurations/**: pasta compartilhada entre todos os subprojetos.

```
mkdir -p apps/api apps/adm apps/mobile apps/web packages/configurations
```
[def]: #configuração
### 3. **Configuração do Turborepo**

Edite o `package.json` principal para adicionar o Turborepo:

```jsx
{
  "name": "tubo1",
  "private": true,
  "devDependencies": {
    "turbo": "^2.2.1"
  },
  "scripts": {
    "dev:api": "turbo run dev --filter=api",
    "dev:adm": "turbo run dev --filter=adm",
    "dev:mobile": "turbo run start --filter=mobile",
    "dev:web": "turbo run dev --filter=web",
    "dev": "turbo run dev",
    "build": "turbo run build"
  },
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "packageManager": "npm@10.9.0"
}
```

Adicionamos a propriedade `"workspaces"`, que permite que o Turborepo reconheça os subprojetos.

Em "packageManager": "npm@10.9.0", é necessário atribuir a versão coreta do npm, para tanto use npm —version e atualize o conteúdo de "packageManager", com a versão correta, caso você esteja usando uma versão diferente

```jsx
C:\turborepo>npm --version
10.9.0
```
[def]: #api
### 4. **Criação do Subprojeto API (Node.js + Express)**

Entre na pasta `apps/api` e inicialize o projeto:

```jsx
cd apps/api
npm init -y
```

Instale o **Express**:

```jsx
npm install express
```

Crie um arquivo `index.js`:

```jsx
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('API está funcionando!');
});

app.listen(port, () => {
  console.log(`API rodando em http://localhost:${port}`);
});
```
[def]: #adm
### 5. **Criação do Subprojeto ADM (React.js)**

Vá para a pasta `apps/adm` e inicialize um projeto React com **Vite**:

```jsx
cd ../../apps/adm
npm create vite@latest .
```

Escolha **React** como o template e siga as instruções.
[def]: #mobile
### 6. **Criação do Subprojeto Mobile (React Native)**

Entre na pasta `apps/mobile` e inicialize o projeto React Native com **Expo**:

```jsx
cd ../../apps/mobile
npx expo init .
√ Choose a template: » blank               a minimal app as clean as an empty canvas
✔ Downloaded template.
📦 Using npm to install packages.
✔ Installed JavaScript dependencies.
✅ Your project is ready!

To run your project, run one of the following npm commands.

- npm start # you can open iOS, Android, or web from here, or run them directly with the commands below.       
- npm run android
- npm run ios # requires an iOS device or macOS for access to an iOS simulator
- npm run web
```
```jsx
npm create vite@latest .
> npx
> create-vite .

√ Select a framework: » React
√ Select a variant: » JavaScript

Scaffolding project in C:\workspaces\causaGanha\testes\tubo1\apps\adm...

Done. Now run:

  npm install
  npm run dev

Escolha a opção de template em branco.
```

Para projetos em react native é necessário fazer um ajuste adicional para que o arquivo inicial do projeto, por exemplo App.js possa ser encontrado.

Estratégia 1:
alterar o endereço do import ao App.js no arquivo 
..\..\node_modules\expo\AppEntry.js
```jsx
import registerRootComponent from 'expo/build/launch/registerRootComponent';
import App from '../../apps/mobile/App';

//import App from '../../App';  // This is the original line

registerRootComponent(App);
```
Estratégia 2:
usar o arquivo index.js em substiuição ao AppEntry.js
```jsx
import registerRootComponent from 'expo/build/launch/registerRootComponent';
import App from './App'; 
registerRootComponent(App);
```
Se você opniar por essa estratégia, mais um ajuste precisa ser realizado no arquivo package.json do apps/mobile retirando a menção ao AppEntry.js
```jsx
{
  "name": "mobile",
  "version": "1.0.0",
  "main": "expo/AppEntry.js",  // alterar essa linha para:
  "main": "index.js", // linha correta
```  
[def]: #next
### 7. **Criação do Subprojeto Web (Next.js)**

Entre na pasta `apps/web` e inicialize o projeto Next.js:

```jsx
cd ../../apps/web
npx create-next-app .
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to customize the default import alias (@/*)? ... No / Yes ```jsx
Creating a new Next.js app in C:\workspaces\causaGanha\testes\tubo1\apps\web.
Using npm.

Initializing project with template: app-tw


Installing dependencies:
- react
- react-dom
- next

Installing devDependencies:
- postcss
- tailwindcss
- eslint
- eslint-config-next
```
[def]: #packages
### 8. **Pasta Compartilhada (Configurations)**

Agora, crie uma pasta `configurations` dentro de `packages` para armazenar configurações compartilhadas, como variáveis de ambiente, temas, etc.

Por exemplo, crie um arquivo `config.js`:

```jsx
// packages/configurations/config.js
module.exports = {
  appName: 'Meu Monorepo',
  apiUrl: 'http://localhost:3000',
};
```
[def]: #shared
### 9. **Configurar Importação Compartilhada**

Agora, para que os subprojetos possam usar essa configuração compartilhada, adicione no `package.json` de cada subprojeto a seguinte linha para a dependência interna:

```jsx
"dependencies": {
  "configurations": "*"
}
```

E nos arquivos de código dos subprojetos, importe a configuração:

```jsx
const config = require('configurations/config');
console.log(config.appName);
```
[def]: #config
### 10. **Configuração dos Scripts no Turborepo**

No `package.json` principal, adicione scripts para rodar os subprojetos de forma paralela com Turborepo:

```jsx
{
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build"
  }
}
```

Em seguida, nos `package.json` de cada subprojeto (como `apps/api/package.json`, `apps/adm/package.json`, etc.), adicione os respectivos scripts:

Para o **API**:

```jsx
"scripts": {
  "dev": "node index.js"
}
```

Para o **ADM** (React):

```jsx
"scripts": {
  "dev": "vite"
}
```

Para o **Mobile** (React Native):

```jsx
"scripts": {
  "dev": "expo start"
}
```

Para o **Web** (Next.js):

```jsx
"scripts": {
    "dev:api": "turbo run dev --filter=api",
    "dev:adm": "turbo run dev --filter=adm",
    "dev:mobile": "turbo run start --filter=mobile",
    "dev:web": "turbo run dev --filter=web",
    "dev": "turbo run dev",
    "build": "turbo run build"
}
```
[def]: #turbo
### 11. **Configurar o turbo.json**

Se ainda não existir esse arquivo, crie-o agora, ou edite o seu conteúdo e adicione a configuração mínima necessária. Abaixo está um exemplo básico de configuração para seu monorepo:

```jsx
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "outputs": ["dist/**"]
    },
    "dev": {
      "cache": false
    }
  }
}
```

Explicação:

- **`build`**: define o pipeline de build, onde o Turborepo observará a pasta `dist/**` para cada subprojeto (caso eles gerem arquivos de build).
- **`dev`**: define o pipeline de desenvolvimento. Aqui, `cache: false` desativa o cache para o ambiente de desenvolvimento, garantindo que sempre rode as últimas versões sem interferências.
[def]: #run
### 12. **Rodando o Projeto**

Agora, execute o monorepo usando o comando:

```jsx
npm run dev
```
#### 12.1 executar todos os projetos simultanemamente
Se tudo estiver correto, ao executar o turbo repo você terá o seguinte feliz resultado:

```jsx
c:/turborepo> turbo run dev
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/aec15188-dc50-45a2-b056-863cd7969c92/4e120411-e51e-4af2-b557-e839be4e1708/image.png)

se você está vendo uma resposta parecida com essa acima, parabéns a sua aplicação no padrão monorepo já está funcionando.

agora você poderá ver os projetos em execução a partir das urls mencionadas:

- api: [http://localhost:3000](http://localhost:3000/)
- adm: http://localhost:5173/
- web: [http://localhost:3001](http://localhost:3001/)
- mobile: http://localhost:8081/ *

* obs: no caso específico da aplicação em react native usando o expo, perceba que embora demore um pouco mais, a url acaba sendo exibida, nesse caso é [http://localhost:8081](http://localhost:8081/). Ao clicar nesse link você perceberá que um pequeno obstáculo possa ocorrer, para que um app react native seja executado no navegador, será necessário instalar a dependência específica para isso react-native-web, veja abaixo o tópico específico sobre isso (trabalhando com o react native web) 

#### 12.2 executando cada projeto individualmente

Para executar os subprojetos individualmente a partir da raiz do monorepo com **Turborepo**, você pode utilizar o comando `turbo run` com o filtro `--filter` para especificar qual subprojeto deseja rodar.

Aqui está o passo a passo para rodar cada subprojeto individualmente, incluindo o projeto mobile com o emulador Android:

##### a) verificar Scripts nos Subprojetos

Primeiro, cada subprojeto deve ter os scripts apropriados no seu respectivo `package.json`. Por exemplo:

##### b) subprojeto mobile:

No arquivo `apps/mobile/package.json`, garanta que o script para iniciar o projeto React Native esteja configurado:

```jsx
{
  "name": "mobile",
  "version": "1.0.0",
  "scripts": {
    "start": "expo start",
    "android": "expo run:android",
    "ios": "expo run:ios"
  }
}
```

Para os outros subprojetos, como **api** ou **adm**, eles também devem ter scripts, como:

```jsx
{
  "scripts": {
    "dev": "node index.js"
  }
}
```

##### c) executar cada Subprojeto

Para rodar cada subprojeto separadamente, use o seguinte comando a partir da raiz do monorepo, usando o `--filter`:

- **Para rodar a aplicação mobile (React Native)** no Android, já com o emulador aberto:

```jsx
turbo run android --filter=mobile
```

Isso executará o comando `expo run:android` dentro do subprojeto `mobile`.

- **Para rodar o subprojeto `api` (Node.js)**, por exemplo:

```jsx
turbo run dev --filter=api
```

Esse comando executará o script `dev` dentro do subprojeto `api`.

- **Para rodar o subprojeto `adm` (React.js Admin Panel)**:

```jsx
turbo run dev --filter=adm
```

### **Adicionar Scripts Personalizados na Raiz**

Para facilitar a execução de cada subprojeto individualmente, você pode adicionar scripts no `package.json` da raiz do projeto. Por exemplo:

No `package.json` da raiz, adicione (caso esses scripts ainda não existam):

```jsx
{
  "scripts": {
   "dev:api": "turbo run dev --filter=api",
    "dev:adm": "turbo run dev --filter=adm",
    "dev:mobile": "turbo run android --filter=mobile",  
    "dev:web": "turbo run dev --filter=web",
    "dev": "turbo run dev",
    "build": "turbo run build"
  }
}
```

Com isso, você poderá rodar cada subprojeto diretamente da raiz com os comandos:

```jsx
npm run dev:api

npm run dev:mobile

npm run dev:adm

npm run dev:web
```

Essa abordagem permite que você execute qualquer subprojeto individualmente a partir da raiz do monorepo usando **Turborepo**, e no caso do mobile, você já pode rodar diretamente no emulador com o comando para Android.

[def]: #problems1
### 13. Possíveis problemas

Realmente existe uma gama significativa de problemas que podem ocorrer impedindo a execução do seu projeto, mas os principais são:

- erros referentes à versões do turborepo
- erros referentes à versões do npm e expo cli p/projetos react native ([vide artigo **The New Expo CL](https://blog.expo.dev/the-new-expo-cli-f4250d8e3421)I)**

A seguir, um possível erro referente a versão do turborepo:

```jsx
× found pipeline field instead of tasks
╭─[turbo.json:2:1]
  2 │         "$schema": "https://turbo.build/schema.json",
  3 │ ╭─▶     "pipeline": {
  4 │ │         "build": {
  5 │ │           "outputs": ["dist/**"]
  6 │ │         },
  7 │ │         "dev": {
  8 │ │           "cache": false
  9 │ │         }
 10 │ ├─▶     }
    · ╰──── rename pipeline field to tasks
 11 │       }
    ╰────
  help: changed in 2.0: pipeline has been renamed to tasks
```

Esse erro ocorre porque, nas versões mais recentes do **Turborepo** (a partir da versão 2.0), o campo `pipeline` foi renomeado para **`tasks`**. Para resolver isso, você precisa apenas atualizar o arquivo `turbo.json`, substituindo `pipeline` por `tasks`.

Checar a versão do turbo repo

```jsx
c:/turborepo> npx turbo --version
2.2.1
```

Com base na versão, modifique o arquivo `turbo.json`, nesse exemplo temos a versão **2.2.1**

**Para versões anteriores à 2.0**, o arquivo deverá conter `pipeline`:

```jsx
{
  "$schema": "https://turbo.build/schema.json",
  "pipeline": {
    "build": {
      "outputs": ["dist/**"]
    },
    "dev": {
      "cache": false
    }
  }
}
```

**Para versões 2.0 ou superiores**, o arquivo deverá conter `tasks`:

```jsx
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "outputs": ["dist/**"]
    },
    "dev": {
      "cache": false
    }
  }
}
```

Se você desejar atualizar a versão do turborepo (recomendado):

**opção 1: [(vide tutorial do turborepo](https://turbo.build/repo/docs/crafting-your-repository/upgrading)):**

```jsx
npx @turbo/codemod migrate
```

[Upgrading | Turborepo](https://turbo.build/repo/docs/crafting-your-repository/upgrading)

**opção 2 (opção usada nesse tutorial):**

1. alterar o package.json (da raiz do projeto) em "devDependencies"

```jsx
{
  "name": "t1",
  "private": true,
  "devDependencies": {
    "turbo": "^2.2.1"    // trecho alterado para refletir a versão correta do turbo repo (obs: apagar esse comentário)
  },
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build"
  },
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "packageManager": "npm@10.9.0"
}
```

b. atualizar o node_modules

```jsx
c:/turborepo> npm i
```

c. alterar o **`turbo.json`**

Abra o arquivo `turbo.json` e substitua o campo `pipeline` por `tasks`, como mostrado abaixo:

```jsx
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "outputs": ["dist/**"]
    },
    "dev": {
      "cache": false
    }
  }
}
```
[def]: #problems2
### 14. Como contornar / evitar alguns problemas já conhecidos

- fixar as versões de todos os plugins/recursos usados no projeto (package.json)
- usar o yarn no lugar do npm sempre que possível
- usar versões do turborepo fixas dentro do projeto, evitar usar a instalação global do turborepo como padrão para os seus projetos, possibilitando que cada projeto siga com a sua própria versão do turborepo

14.1 fixar as versões de todos os plugins/recursos usados no projeto (package.json) 

Fixar as versões de dependências e plugins em um projeto monorepo utilizando o **Turborepo** garante que todos os subprojetos utilizem exatamente as mesmas versões de bibliotecas, evitando problemas relacionados a atualizações inesperadas e garantindo consistência no desenvolvimento.

Aqui estão os pontos importantes dessa abordagem:

### **Pontos Positivos:**

1. **Consistência**: Ao fixar as versões, você garante que todos os desenvolvedores e pipelines de CI/CD usarão as mesmas versões de bibliotecas, evitando discrepâncias entre ambientes.
2. **Previsibilidade**: Novas versões de pacotes não introduzirão mudanças inesperadas, o que pode reduzir bugs causados por quebras de compatibilidade.
3. **Facilidade de Debug**: Como todos os subprojetos usam as mesmas versões, fica mais fácil reproduzir e corrigir problemas que possam surgir.
4. **Controle de Atualizações**: Você pode controlar quando e como atualizar dependências, planejando as mudanças com mais segurança, testando tudo antes de incorporar a nova versão.

### **Pontos Negativos:**

1. **Manutenção Manual**: Fixar versões requer manutenção manual para atualizá-las. Isso pode levar a um "acúmulo de dívidas técnicas" se as versões ficarem muito desatualizadas, e atualizações podem se tornar mais complexas.
2. **Perda de Novos Recursos**: Você pode perder correções de bugs ou novos recursos, caso decida não atualizar frequentemente.
3. **Conflitos**: Se algum pacote estiver desatualizado, pode haver incompatibilidade com outras bibliotecas ou ferramentas que precisam de versões mais novas.

### **14.1.2 Como Implementar a Abordagem de Fixar Versões**

Aqui estão os passos práticos para implementar o bloqueio de versões no seu projeto monorepo com Turborepo:

### 1. **Fixar as Versões no `package.json`**

Em cada subprojeto, as dependências no arquivo `package.json` devem ter versões exatas (sem operadores como `^` ou `~`).

```jsx
Por exemplo, em vez de:
"express": "^4.18.0"

Use:
"express": "4.18.0"
```

### 2. **Utilizar o `package-lock.json` ou `yarn.lock`**

Se estiver usando **npm**, o arquivo `package-lock.json` será gerado automaticamente ao rodar `npm install`. Ele armazena todas as versões exatas de cada dependência e suas dependências internas, fixando toda a árvore de dependências.

Se estiver usando **yarn**, o arquivo `yarn.lock` faz o mesmo papel.

Isso é crucial para garantir que a mesma versão de cada pacote seja instalada em qualquer máquina que clonar o projeto ou durante o CI/CD.

### 3. **Monorepo Centralizado: Gerenciamento de Dependências Compartilhadas**

No contexto do Turborepo, você pode compartilhar dependências entre os subprojetos para garantir que todos usem exatamente a mesma versão de uma biblioteca. O Turborepo aproveita o uso de **workspaces** do npm ou yarn para isso.

Por exemplo, no `package.json` principal (na raiz do monorepo), você pode definir dependências que serão compartilhadas entre os subprojetos:

```jsx
{
  "name": "meu-monorepo",
  "private": true,
  "devDependencies": {
    "turbo": "^1.5.0",
    "express": "4.18.0",
    "react": "18.2.0",
    "react-native": "0.71.0"
  },
  "workspaces": [
    "apps/*",
    "packages/*"
  ]
}

```

Assim, todas as dependências são resolvidas centralmente e usadas pelos subprojetos, garantindo que todos usem as mesmas versões.

### 4. **Automatizar a Verificação de Versões com Ferramentas de Auditoria**

Ferramentas como **Dependabot**, **Renovate** ou **npm outdated** podem ser configuradas para te alertar quando houver novas versões de dependências. Você poderá avaliar a segurança e planejar atualizações de forma controlada.

### **Como Lidar com Atualizações de Dependências**

1. **Planeje Atualizações Periódicas**: Decida momentos específicos para revisar e atualizar as dependências, testando rigorosamente após cada atualização.
2. **Testes Automatizados**: Antes de atualizar as dependências, assegure-se de que tenha uma boa cobertura de testes automatizados. Isso minimiza o risco de quebras durante as atualizações.
3. **Snapshot das Dependências**: Ferramentas como `npm shrinkwrap` podem ser utilizadas para criar um snapshot das dependências, garantindo a replicação exata dos pacotes em qualquer ambiente.

### Exemplo de Fixação e Gerenciamento de Dependências:

- **Passo 1**: No `package.json` da raiz do monorepo:

```jsx
{
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "devDependencies": {
    "express": "4.18.0",
    "react": "18.2.0",
    "react-native": "0.71.0",
    "turbo": "1.5.0"
  }
}

```

**Passo 2**: No `apps/api/package.json`:

```jsx
{
  "name": "api",
  "version": "1.0.0",
  "devDependencies": {
    "express": "4.18.0"
  }
}
```

obs: usar esse procedimento para todos os subprojetos do monorepo

**Passo 3**: Certifique-se de que o `package-lock.json` ou `yarn.lock` está em dia, rodando:

```jsx
npm install
```

Esses passos garantem que as dependências em todos os subprojetos compartilhem versões exatas, evitando divergências.

trabalhando com o react native web 

Para usar oum projeto reactive native na web, será necessário instalar as dependências específicas

```jsx
npx expo install react-native-web @expo/metro-runtime -- --force
```

use —force se tiver problemas para instalar esse recurso
# Anexos
[def]: #sources
## Fontes
turborepo
node js
express js
react js
react native
vite
next js
npm
yarn
versel

[def]: #templates
## turborepo templates

[Installation | Turborepo](https://turbo.build/repo/docs/getting-started/installation)

turborepo docs

[turborepo/examples at main · vercel/turborepo](https://github.com/vercel/turborepo/tree/main/examples)

guithub

| **Name**                                                                                          | **Description**                                                                                                                           |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| [basic](https://github.com/vercel/turbo/tree/main/examples/basic)                                 | Exemplo mínimo do Turborepo para aprender os fundamentos.                                                                                 |
| [design-system](https://github.com/vercel/turbo/tree/main/examples/design-system)                 | Unifique a aparência do seu site compartilhando um sistema de design em vários aplicativos.                                               |
| [kitchen-sink](https://github.com/vercel/turbo/tree/main/examples/kitchen-sink)                   | Quer ver um exemplo mais aprofundado? Inclui múltiplas estruturas, tanto frontend quanto backend.                                         |
| [non-monorepo](https://github.com/vercel/turbo/tree/main/examples/non-monorepo)                   | Exemplo de uso do Turborepo em um único projeto sem espaços de trabalho                                                                   |
| [with-changesets](https://github.com/vercel/turbo/tree/main/examples/with-changesets)             | Monorepo Next.js simples pré-configurado para publicar pacotes via Changesets                                                             |
| [with-docker](https://github.com/vercel/turbo/tree/main/examples/with-docker)                     | Monorepo com uma API Express e um aplicativo Next.js implantado com Docker utilizando turbo prune                                         |
| [with-gatsby](https://github.com/vercel/turbo/tree/main/examples/with-gatsby)                     | Monorepo com um aplicativo Gatsby.js e um Next.js, ambos compartilhando uma biblioteca de IU                                              |
| [with-prisma](https://github.com/vercel/turbo/tree/main/examples/with-prisma)                     | Monorepo com um aplicativo Next.js totalmente configurado com Prisma                                                                      |
| [with-react-native-web](https://github.com/vercel/turbo/tree/main/examples/with-react-native-web) | Monorepo React Native e Next.js simples com uma biblioteca de IU compartilhada                                                            |
| [with-rollup](https://github.com/vercel/turbo/tree/main/examples/with-rollup)                     | Monorepo com um único aplicativo Next.js compartilhando uma biblioteca de IU empacotada com Rollup                                        |
| [with-svelte](https://github.com/vercel/turbo/tree/main/examples/with-svelte)                     | Monorepo com vários aplicativos SvelteKit compartilhando uma biblioteca de IU                                                             |
| [with-tailwind](https://github.com/vercel/turbo/tree/main/examples/with-tailwind)                 | Monorepo com vários aplicativos Next.js compartilhando uma biblioteca de IU, todos usando Tailwind CSS com uma configuração compartilhada |
| [with-vite](https://github.com/vercel/turbo/tree/main/examples/with-vite)                         | Monorepo com vários aplicativos Vanilla JS agrupados com Vite, compartilhando uma biblioteca de IU                                        |
| [with-vue-nuxt](https://github.com/vercel/turbo/tree/main/examples/with-vue-nuxt)                 | Monorepo com Vue e Nuxt, compartilhando uma biblioteca de IU                                                                              |