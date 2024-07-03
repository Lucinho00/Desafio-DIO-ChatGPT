# Desafio-DIO-ChatGPT
Passo 1: Configuração do Projeto
1. Clone o repositório
Primeiro, clone o repositório existente ou crie um novo diretório para o seu projeto:
# Clone o repositório
$ git clone https://github.com/felipeAguiarCode/node-chatgpt-api

# Vá para o diretório do repositório
$ cd node-chatgpt-api
2. Inicialize o Projeto Node.js
Se o repositório estiver vazio, inicialize um novo projeto Node.js:
# Inicialize um novo projeto Node.js
$ npm init -y

3. Instale as Dependências
Instale as bibliotecas necessárias:
# Instale as dependências
$ npm install express cors dotenv openai nodemon
Passo 2: Configuração do Servidor
1. Estrutura de Diretórios
Crie a seguinte estrutura de diretórios para seguir a arquitetura em camadas (N-Tier):
node-chatgpt-api
│   .env
│   package.json
│   server.js
└───src
    ├───controllers
    │       promptController.js
    ├───routes
    │       promptRoutes.js
    └───services
            openaiService.js
2. Configuração do Arquivo .env
Crie um arquivo .env na raiz do projeto e adicione sua chave de API da OpenAI:
OPENAI_API_KEY=YOUR_OPENAI_API_KEY
PORT=3001


Vamos criar uma API que integra o ChatGPT da OpenAI utilizando Node.js e Express. Vamos seguir as diretrizes e criar um repositório conforme solicitado.

Passo 1: Configuração do Projeto
1. Clone o repositório
Primeiro, clone o repositório existente ou crie um novo diretório para o seu projeto:

bash
Copiar código
# Clone o repositório
$ git clone https://github.com/felipeAguiarCode/node-chatgpt-api

# Vá para o diretório do repositório
$ cd node-chatgpt-api
2. Inicialize o Projeto Node.js
Se o repositório estiver vazio, inicialize um novo projeto Node.js:

bash
Copiar código
# Inicialize um novo projeto Node.js
$ npm init -y
3. Instale as Dependências
Instale as bibliotecas necessárias:

bash
Copiar código
# Instale as dependências
$ npm install express cors dotenv openai nodemon
Passo 2: Configuração do Servidor
1. Estrutura de Diretórios
Crie a seguinte estrutura de diretórios para seguir a arquitetura em camadas (N-Tier):

bash
Copiar código
node-chatgpt-api
│   .env
│   package.json
│   server.js
└───src
    ├───controllers
    │       promptController.js
    ├───routes
    │       promptRoutes.js
    └───services
            openaiService.js
2. Configuração do Arquivo .env
Crie um arquivo .env na raiz do projeto e adicione sua chave de API da OpenAI:

makefile
Copiar código
OPENAI_API_KEY=YOUR_OPENAI_API_KEY
PORT=3001
3. Configuração do Servidor Express
Crie um arquivo server.js na raiz do projeto com o seguinte conteúdo:
const express = require('express');
const cors = require('cors');
const dotenv = require('dotenv');
const promptRoutes = require('./src/routes/promptRoutes');

dotenv.config();

const app = express();
const port = process.env.PORT || 3001;

app.use(cors());
app.use(express.json());
app.use('/api', promptRoutes);

app.listen(port, () => {
    console.log(`Servidor rodando em http://localhost:${port}`);
});

4. Configuração do Serviço OpenAI
Crie o arquivo src/services/openaiService.js:
const { Configuration, OpenAIApi } = require('openai');
const dotenv = require('dotenv');

dotenv.config();

const configuration = new Configuration({
    apiKey: process.env.OPENAI_API_KEY,
});

const openai = new OpenAIApi(configuration);

const getChatGPTResponse = async (prompt) => {
    const response = await openai.createCompletion({
        model: 'text-davinci-003',
        prompt: prompt,
        max_tokens: 150,
    });
    return response.data.choices[0].text.trim();
};

module.exports = { getChatGPTResponse };

5. Configuração do Controller
Crie o arquivo src/controllers/promptController.js:

const { getChatGPTResponse } = require('../services/openaiService');

const handlePrompt = async (req, res) => {
    const { prompt } = req.body;
    try {
        const response = await getChatGPTResponse(prompt);
        res.status(200).json({ response });
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: 'Erro ao obter resposta do ChatGPT' });
    }
};

module.exports = { handlePrompt };
6. Configuração das Rotas
Crie o arquivo src/routes/promptRoutes.js:
const express = require('express');
const { handlePrompt } = require('../controllers/promptController');

const router = express.Router();

router.post('/prompt', handlePrompt);

module.exports = router;

Passo 3: Scripts do NPM
Atualize o package.json para incluir um script para rodar o servidor em modo de desenvolvimento com o nodemon:
{
  "name": "node-chatgpt-api",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.0.1",
    "express": "^4.17.1",
    "openai": "^2.0.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.12"
  }
}
Passo 4: Executar o Servidor
Para rodar o servidor em ambiente de desenvolvimento, use o comando:
$ npm run dev

Conclusão
Agora você tem uma API RESTful criada com Node.js que integra o ChatGPT da OpenAI. Você pode enviar prompts para o endpoint /api/prompt e obter respostas geradas pelo modelo de linguagem.
