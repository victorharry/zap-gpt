*Imagem gerada com a IA do Midjourney*
![Imagem gerada no Midjourney](https://miro.medium.com/max/4800/1*FyJQWP0fVMMHXhEa7M0kOQ.webp)

Este guia irá explicar como integrar o modelo de linguagem GPT-3 utilizado no ChatGPT com o WhatsApp. A integração permitirá que usuários interajam com o ChatGPT através de mensagens de texto no WhatsApp, fornecendo respostas automatizadas baseadas em suas perguntas e comandos. O guia discutirá os passos necessários desde a configuração até a integração. Vamos usar o Node.js com Javascript para criar o projeto e a biblioteca venom-bot para a integração com o Whatsapp.

![Chat GPT Logo](https://miro.medium.com/max/4800/1*HhDoR-hLcObJXSwDDEar4g.webp)

## Vantagens de integrar o modelo GPT-3 com Whatsapp

1. Conforto de utilizar a IA no Whatsapp
2. Qualquer pessoa pode mandar os comandos para a IA no privado ou em grupos que o contato que esteja rodando este serviço estiver.
3. O filtro para perguntas politicamente incorretas está desligado ao comunicar diretamente com a API. ~~Seria isso uma vantagem?~~ 👀

Teste do filtro feito no próprio site do Chat GPT:
![Chat GPT Teste Filtro](https://miro.medium.com/max/4800/1*98irGe0GAlvlhkErKKw6iQ.webp)

Teste do filtro feito no Whatsapp utilizando a API:
![Chat GPT Teste Filtro Whatsapp](https://miro.medium.com/max/584/1*bZ6Wfjv7LA5kzUHOJnjMhQ.png)

## 1. Criando o projeto
O Node.js é uma plataforma de desenvolvimento baseada em Javascript que permite criar aplicações como APIs e serviços. Ele foi baseado no interpretador V8 do Google permitindo assim rodar diretamente no servidor sem necessidade de um navegador para sua execução. Diferente do uso tradicional do Javascript, que é restrito ao client-side, o Node.js permite aproveitar as vantagens do Javascript no desenvolvimento de aplicações no server-side.

![Javascript Power](https://miro.medium.com/max/559/1*nSfk7VzjCNeBwzlpuf1n1g.jpeg)

### Passo 1.1: Instalando o Node.js e o npm
A primeira coisa que você precisa fazer é instalar o Node.js em seu computador. Você pode baixar o instalador no site oficial do Node.js (https://nodejs.org/en/download/) e seguir as instruções de instalação. Uma vez que você instalou o Node.js, você também terá o npm instalado automaticamente, o npm é o gerenciador de pacotes do Node.js ele permite instalar e gerenciar bibliotecas e dependências. Para verificar se você tem o Node.js e o npm instalados corretamente, abra o prompt de comando e digite “node -v” e “npm -v” para ver as versões instaladas.

### Passo 1.2: Criando um novo projeto
Depois de ter o Node.js e o npm instalados, você pode criar um novo projeto Node.js. Abra o prompt de comando e navegue até a pasta onde você deseja criar o projeto, digite “npm init” para iniciar o processo de criação, isso criará um arquivo package.json que contém as informações do projeto, incluindo nome, versão, dependências e scripts. Ele é usado pelo npm para gerenciar as dependências do projeto.

```
# Primeiro crie a sua pasta com o nome do projeto
mkdir zap-gpt

# Entre na pasta criada 
cd zap-gpt

# Crie o arquivo package.json com o comando
npm init
```

Agora vamos adicionar em nosso package.json o “type” como “module”, assim a gente vai poder usar a syntax de import do ES6 Modules.

```
{
  "name": "zap-gpt",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

## 2. Instalando e configurando o venom-bot
Venom é uma dependência desenvolvida com JavaScript para criar serviços para WhatsApp como atendimento ao cliente automatizado, envio de mídia, reconhecimento de frases baseado em inteligência artificial e todos os tipos de arquitetura de design para WhatsApp.

![Venom Bot Image](https://miro.medium.com/max/1400/1*cJSZ1y1RP9FrQgUZKag7vw.webp)

### Passo 2.1: Instalação
Agora que voce já tem o arquivo package.json, já pode instalar as dependências do seu projeto. Você vai instalá-lo digitando “npm install venom-bot” no prompt de comando.

```
# Instale venom-bot como uma dependencia do seu projeto
npm install venom-bot
```

### Passo 2.2: Configuração
Não vou me atentar na estruturação das pastas neste guia, então vamos apenas criar um arquivo index.js dentro na raiz do projeto.

![Estrutura de pastas index.js](https://miro.medium.com/max/640/1*_ah8dQeNHiuLlcrn4IvKKw.webp)

No nosso arquivo index iremos criar uma nova sessão e chamar nossa função start() após a conexão ser feita com sucesso. Para que a gente possa observar qualquer evento disparado de mensagem no nosso whatsapp iremos chamar a função onAnyMessage do client para que ele observe todas as mensagens enviadas incluindo as nossas, e passaremos como parametro uma function como callback para que seja executada sempre que uma nova mensagem for enviada, validaremos então o conteúdo da mensagem, se o conteúdo for “hello” iremos enviar uma mensagem de volta “🤖 world 🌎”.

```
import { create } from 'venom-bot'

create({
    session: 'chat-gpt',
    multidevice: true
})
    .then((client) => start(client))
    .catch((erro) => {
        console.log(erro)
    })

async function start(client) {
    const botText = "🤖 world 🌎"
    // Da um console.log em message depois, tem muita coisa bacana
    client.onAnyMessage((message) => {
        if (message.body.toLowerCase() === "hello") {
            // message.from é o número do usuário que enviou a msg "hello"
            client.sendText(message.from, botText)
        }
    })
}
```

### Passo 2.3: Testando a implementação
Execute o projeto com o comando “node .” em seu terminal, assim ele irá executar o arquivo nomeado como index no diretorio onde o comando foi executado. Após aguardar alguns instantes será gerado um QR Code que deve ser lido pelo aplicativo do Whatsapp. Em seu aplicativo toque em “Mais opções” ou “Configurações”, selecione “Aparelhos conectados” e depois selecione a opção “Conectar um aparelho” para ler o QR Code gerado.

![Inicialização do projeto](https://miro.medium.com/max/640/1*wZGVeFhTmdzFV0b0deCBkg.webp)

Após ler o QR Code irá aparecer no terminal o status *“✓ [instance: Chat-GPT]: Connected”* indicando que a conexão foi bem sucedida e irá criar uma pasta no seu projeto chamada tokens, esta pasta guarda a sessão da conexão, dessa forma voce não precisará ler o QR Code toda vez que inicializar o projeto.

![Estrutura das pastas após instalação do venom](https://miro.medium.com/max/614/1*3jGekQPj3WDS4BKR9d4kdA.webp)

Vamos agora testar nosso humilde script e ver se deu tudo certinho com nossa integração com o Venom!

![Teste do venom](https://miro.medium.com/max/720/1*CA7Dn78ZSo2M6EU5LIhenw.webp)

## 3. Instalando e configurando o “Chat GPT”
Nada melhor do que o nosso queridíssimo Chat GPT para nos explicar o significado de “GPT”.

> GPT é uma sigla para “Generative Pre-trained Transformer”, é um modelo de linguagem treinado pela OpenAI. Ele é capaz de gerar texto de forma autônoma, com base em um grande conjunto de dados de texto pré-treinado. Ele pode ser usado para tarefas como preenchimento automático, geração de texto, tradução automática, entre outras. Ele é uma das tecnologias de IA de linguagem mais avançadas disponíveis atualmente e é utilizado em uma variedade de aplicações, incluindo chatbots, assistentes virtuais e sistemas de geração automática de conteúdo.

![Chat GPT Logo](https://miro.medium.com/max/720/1*04Y9k6eas6R0iLUNjeqIRw.webp)

### Passo 3.1: Pegar a API Key e Organization ID na OpenAI
A primeira coisa que precisamos aqui é da api key da openai, uma chave para autorização de envio das nossas requisições. Entre neste link para pegar sua chave https://beta.openai.com/account/api-keys, é bem auto explicativo.

![API Key na open api](https://miro.medium.com/max/720/1*SYxpE_Es5MSC3ORTQPySNg.webp)

Para pegar o organization ID também é bem fácil, é só acessar este link e copiar o seu ID https://beta.openai.com/account/org-settings

![Organization ID](https://miro.medium.com/max/640/1*jdI2EmPw9G0WAoL-rdBcIg.webp)

### Passo 3.2: Instalação
Agora da mesma forma que fizemos com o venom faremos com a dependência da openai, instale-a utilizando o comando “npm install openai” em seu terminal na raiz do projeto.

```
# Instale openai como uma dependencia do seu projeto
npm install openai
```

Para evitarmos futuros problemas como o vazamento da nossa api key e organization id vamos instalar também uma depencia chamada dotenv, que serve para criarmos variáveis de ambiente separadas do código.

```
# Instale dotenv como uma dependencia do seu projeto
npm install dotenv
```

### Passo 3.3: Configuração
Vamos começar configurando nossas variáveis de ambiente com o dotenv. Na raiz do seu projeto crie um arquivo chamado “.env” nele colocaremos quaisquer dados sensíveis que precisaremos utilizar em nosso projeto.

![Estrutura das pastas .env](https://miro.medium.com/max/620/1*cPviWtCaiAtkODrqcPM_3A.webp)

Dentro do arquivo iremos criar uma variável chamada OPENAI_KEY e colocaremos nossa api key aqui.

```
OPENAI_KEY=SUA-API-KEY
ORGANIZATION_ID=SEU-ORGANIZATION-ID
PHONE_NUMBER=SEU-NUMERO exemplo -> 553144448888@c.us
```

Em nosso arquivo index.js vamos carregar nossas variáveis para podermos utiliza-las e instanciar a configuração da openai.

```
import { create } from 'venom-bot'
import * as dotenv from 'dotenv'
import { Configuration, OpenAIApi } from "openai"

dotenv.config()

/* Agora podemos chamar nossas variáveis de ambiente
 * process.env.OPENAI_KEY
 * process.env.ORGANIZATION_ID
 * process.env.PHONE_NUMBER
 */ 
const configuration = new Configuration({
    organization: process.env.ORGANIZATION_ID,
    apiKey: process.env.OPENAI_KEY,
});
```

Após isso iremos criar 3 funções, a primeira *getDavinciResponse()* será para fazer chamadas para o davinci-003 que irá gerar as respostas em texto:

```
const getDavinciResponse = async (clientText) => {
    const options = {
        model: "text-davinci-003", // Modelo GPT a ser usado
        prompt: clientText, // Texto enviado pelo usuário
        temperature: 1, // Nível de variação das respostas geradas, 1 é o máximo
        max_tokens: 4000 // Quantidade de tokens (palavras) a serem retornadas pelo bot, 4000 é o máximo
    }

    try {
        const response = await openai.createCompletion(options)
        let botResponse = ""
        response.data.choices.forEach(({ text }) => {
            botResponse += text
        })
        return `Chat GPT 🤖\n\n ${botResponse.trim()}`
    } catch (e) {
        return `❌ OpenAI Response Error: ${e.response.data.error.message}`
    }
}
```

A segunda função será para a DALL-E *getDalleResponse()* que irá gerar nossas imagens:

```
const getDalleResponse = async (clientText) => {
    const options = {
        prompt: clientText, // Descrição da imagem
        n: 1, // Número de imagens a serem geradas
        size: "1024x1024", // Tamanho da imagem
    }

    try {
        const response = await openai.createImage(options);
        return response.data.data[0].url
    } catch (e) {
        return `❌ OpenAI Response Error: ${e.response.data.error.message}`
    }
}
```

A tarceira e ultima *commands()*, será para mapear os comandos que chegam como input do usuário:

```
const commands = (client, message) => {
    const iaCommands = {
        davinci3: "/bot",
        dalle: "/img"
    }

    let firstWord = message.text.substring(0, message.text.indexOf(" "));

    switch (firstWord) {
        case iaCommands.davinci3:
            const question = message.text.substring(message.text.indexOf(" "));
            getDavinciResponse(question).then((response) => {
                /*
                 * Faremos uma validação no message.from
                 * para caso a gente envie um comando
                 * a response não seja enviada para
                 * nosso próprio número e sim para 
                 * a pessoa ou grupo para o qual eu enviei
                 */
                client.sendText(message.from === process.env.BOT_NUMBER ? message.to : message.from, response)
            })
            break;

        case iaCommands.dalle:
            const imgDescription = message.text.substring(message.text.indexOf(" "));
            getDalleResponse(imgDescription, message).then((imgUrl) => {
                client.sendImage(
                    message.from === process.env.BOT_NUMBER ? message.to : message.from,
                    imgUrl,
                    imgDescription,
                    'Imagem gerada pela IA DALL-E 🤖'
                )
            })
            break;
    }
}
```

Por fim iremos listar dentro do nosso callback do evento *onAnyMessage()* a função *commands()*. O nosso index.js ficará exatamente como mostrado abaixo!

```
import { create } from 'venom-bot'
import * as dotenv from 'dotenv'
import { Configuration, OpenAIApi } from "openai"

dotenv.config()

create({
    session: 'Chat-GPT',
    multidevice: true
})
    .then((client) => start(client))
    .catch((erro) => {
        console.log(erro);
    });

const configuration = new Configuration({
    organization: process.env.ORGANIZATION_ID,
    apiKey: process.env.OPENAI_KEY,
});

const openai = new OpenAIApi(configuration);

const getDavinciResponse = async (clientText) => {
    const options = {
        model: "text-davinci-003", // Modelo GPT a ser usado
        prompt: clientText, // Texto enviado pelo usuário
        temperature: 1, // Nível de variação das respostas geradas, 1 é o máximo
        max_tokens: 4000 // Quantidade de tokens (palavras) a serem retornadas pelo bot, 4000 é o máximo
    }

    try {
        const response = await openai.createCompletion(options)
        let botResponse = ""
        response.data.choices.forEach(({ text }) => {
            botResponse += text
        })
        return `Chat GPT 🤖\n\n ${botResponse.trim()}`
    } catch (e) {
        return `❌ OpenAI Response Error: ${e.response.data.error.message}`
    }
}

const getDalleResponse = async (clientText) => {
    const options = {
        prompt: clientText, // Descrição da imagem
        n: 1, // Número de imagens a serem geradas
        size: "1024x1024", // Tamanho da imagem
    }

    try {
        const response = await openai.createImage(options);
        return response.data.data[0].url
    } catch (e) {
        return `❌ OpenAI Response Error: ${e.response.data.error.message}`
    }
}

const commands = (client, message) => {
    const iaCommands = {
        davinci3: "/bot",
        dalle: "/img"
    }

    let firstWord = message.text.substring(0, message.text.indexOf(" "));

    switch (firstWord) {
        case iaCommands.davinci3:
            const question = message.text.substring(message.text.indexOf(" "));
            getDavinciResponse(question).then((response) => {
                /*
                 * Faremos uma validação no message.from
                 * para caso a gente envie um comando
                 * a response não seja enviada para
                 * nosso próprio número e sim para 
                 * a pessoa ou grupo para o qual eu enviei
                 */
                client.sendText(message.from === process.env.BOT_NUMBER ? message.to : message.from, response)
            })
            break;

        case iaCommands.dalle:
            const imgDescription = message.text.substring(message.text.indexOf(" "));
            getDalleResponse(imgDescription, message).then((imgUrl) => {
                client.sendImage(
                    message.from === process.env.BOT_NUMBER ? message.to : message.from,
                    imgUrl,
                    imgDescription,
                    'Imagem gerada pela IA DALL-E 🤖'
                )
            })
            break;
    }
}

async function start(client) {
    client.onAnyMessage((message) => commands(client, message));
}
```

### Passo 3.4: Hora de testar!

![Teste da implementação](https://miro.medium.com/max/720/1*0K0tAo1ujQoFa9NG3ClIdw.webp)

## 4. Considerações finais
Se te ajudei de alguma forma já curte esse guia para que ele chegue em mais pessoas e as ajude também!

### 4.1 Dicas
Voce pode subir esse serviço em uma máquina EC2 da AWS Amazon para que ele fique 24/7 ( rodando 24h por dia ) gratuitamente por 1 ano! Assim voce não precisa pagar mais caro na sua conta de luz com seu PC Gamer com RPG ligado a semana toda!

Caso seja do seu interesse se inscreve lá no meu canal, não tem nada ainda mas em breve vou soltar vídeos explicando passo a passo de como fazer essa instalação do projeto em um servidor da EC2, e também criarei outros conteúdos legais como desse post!

[Canal Dev Cansado](https://www.youtube.com/@devcansadodms)

### 4.2 Ta com preguiça e quer um plug and play?
Sei que eu deveria colocar isso no início pra evitar da galera descobrir só depois de ler tudo mas você também deveria ler mais 👍

Vai lá no repositório e da fork: https://github.com/victorharry/zap-gpt

### 4.3 Sugestões
Caso tenha uma ideia de como otimizar o código e deixar ele mais legível  pode deixar aqui comentado ou abre um MR lá no repo que a gente atualiza pra ficar bacana pra todo mundo e pra gente trocar conhecimento também, sempre bom! 

Valeu polvo 🐙

Quem quiser conectar no [Linkedin](https://www.linkedin.com/in/victorhcharry/)

![Comemorações](https://miro.medium.com/max/720/1*T-sT0gthpO-a_GMkERUCMw.webp)

*Deixei a fonte com o link do Medium pois postei lá antes*
