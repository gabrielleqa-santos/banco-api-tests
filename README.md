# Banco API Tests

Projeto de automação de testes para a API REST do [Banco API](https://github.com/juliodelimas/banco-api).

Os testes validam fluxos de autenticação e transferências bancárias, incluindo respostas de sucesso, regras de valor mínimo, consulta de transferências por identificador e paginação.

## Objetivo

Este projeto tem como objetivo verificar automaticamente o comportamento dos principais endpoints REST do Banco API, contribuindo que:

- o login retorne um token para credenciais válidas;
- transferências válidas sejam processadas;
- transferências abaixo do valor mínimo sejam rejeitadas;
- uma transferência possa ser consultada pelo seu identificador;
- a listagem de transferências respeite os parâmetros de paginação.

## Tecnologias utilizadas

| Tecnologia | Finalidade |
| --- | --- |
| JavaScript | Linguagem utilizada na implementação dos testes |
| Node.js | Ambiente de execução do projeto |
| npm | Instalação das dependências e execução dos scripts |
| Mocha | Estrutura e execução dos testes |
| Supertest | Envio das requisições HTTP para a API |
| Chai | Asserções sobre status e conteúdo das respostas |
| dotenv | Carregamento das variáveis definidas no arquivo `.env` |
| Mochawesome | Geração dos relatórios de execução em HTML e JSON |

## Pré-requisitos

Antes de executar os testes, instale:

- [Git](https://git-scm.com/doc);
- [Node.js](https://nodejs.org/docs/latest/api/);
- npm, normalmente instalado com o Node.js;
- o projeto [Banco API](https://github.com/juliodelimas/banco-api), configurado e em execução.

A API REST do projeto utilizado como alvo funciona, por padrão, em `http://localhost:3000`.

## Instalação

Clone este repositório:

```bash
git clone https://github.com/gabrielleqa-santos/banco-api-tests.git
```

Entre no diretório:

```bash
cd banco-api-tests
```

Instale as dependências registradas no `package-lock.json`:

```bash
npm ci
```

Caso o projeto não possua um arquivo `package-lock.json`, utilize:

```bash
npm install
```

## Configuração do arquivo `.env`

O arquivo `.env` não é versionado porque está incluído no `.gitignore`. Cada pessoa que utilizar o projeto deve criá-lo manualmente na raiz do repositório.

Crie o arquivo:

```text
banco-api-tests/
└── .env
```

Adicione a URL base da API REST:

```dotenv
BASE_URL=http://localhost:3000
```

Se a API estiver publicada em outro ambiente, substitua o valor de `BASE_URL`. Exemplo:

```dotenv
BASE_URL=https://endereco-do-ambiente.example
```

Não adicione aspas ou uma barra `/` no final da URL, salvo se o ambiente utilizado exigir esse formato.

> Não envie arquivos `.env`, tokens, senhas ou outras credenciais reais para o repositório.

## Preparação da API

Os testes dependem da API REST em execução e de dados compatíveis com as massas utilizadas.

No projeto `banco-api`, instale as dependências, configure o banco e as variáveis de ambiente conforme o README da própria API. Depois, inicialize a API REST:

```bash
npm run rest-api
```

Antes de iniciar os testes, confirme que a URL configurada em `BASE_URL` está acessível.

## Executando os testes

Para executar todos os arquivos com o padrão `test/**/*.test.js`, utilize:

```bash
npm test
```

O script configurado no `package.json` executa:

```bash
mocha ./test/**/*.test.js --timeout=200000 --reporter mochawesome
```

O tempo limite configurado para cada teste é de `200000` milissegundos.

### Executar um arquivo específico

Para executar somente os testes de login:

```bash
npx mocha ./test/login.test.js --timeout=200000 --reporter mochawesome
```

Para executar somente os testes de transferências:

```bash
npx mocha ./test/transferencia.test.js --timeout=200000 --reporter mochawesome
```

## Relatório Mochawesome

O relatório é gerado automaticamente ao executar `npm test`, pois o script utiliza o reporter `mochawesome`.

Após a execução, os arquivos são disponibilizados em:

```text
mochawesome-report/
├── mochawesome.html
└── mochawesome.json
```

- `mochawesome.html`: relatório visual para consulta no navegador;
- `mochawesome.json`: resultado estruturado da execução.

Para abrir o relatório HTML no macOS:

```bash
open mochawesome-report/mochawesome.html
```

No Windows:

```powershell
start mochawesome-report/mochawesome.html
```

No Linux:

```bash
xdg-open mochawesome-report/mochawesome.html
```

O diretório `mochawesome-report/` está no `.gitignore` e não é enviado ao repositório.

## Estrutura de diretórios

```text
banco-api-tests/
├── fixtures/
│   ├── postLogin.json
│   └── postTransferencias.json
├── helpers/
│   └── autenticacao.js
├── test/
│   ├── login.test.js
│   └── transferencia.test.js
├── .env                         # Criado localmente; não versionado
├── .gitignore
├── package.json
├── package-lock.json
└── README.md
```

### Responsabilidade de cada diretório

- `fixtures/`: armazena as massas utilizadas nos corpos das requisições;
- `helpers/`: contém funções reutilizáveis, como a obtenção do token de autenticação;
- `test/`: contém as suítes e os cenários de teste;
- `mochawesome-report/`: criado após a execução e utilizado para armazenar os relatórios.

## Testes implementados

### Login

- `POST /login`: valida status `200` e o retorno de um token em formato de texto.

### Transferências

- `POST /transferencias`: valida sucesso com status `201` para valor igual ou superior a R$ 10,00;
- `POST /transferencias`: valida status `422` para valor inferior a R$ 10,00;
- `GET /transferencias/{id}`: valida a consulta de uma transferência pelo identificador;
- `GET /transferencias`: valida a quantidade de registros retornados pela paginação.

## Solução de problemas

### `BASE_URL` indefinida

Confira se o arquivo `.env` está na raiz do projeto e contém:

```dotenv
BASE_URL=http://localhost:3000
```

### Falha de conexão (`ECONNREFUSED`)

Verifique se a API REST está em execução e se a porta corresponde ao valor configurado em `BASE_URL`.

### Respostas diferentes das esperadas

Confira se o banco de dados da API possui os registros esperados pelos testes e se as massas em `fixtures/` correspondem ao ambiente executado.

### Token ausente ou inválido

Confirme se o usuário utilizado para login existe na API e se as credenciais da massa de teste são válidas no ambiente.

## Documentação das dependências

- [Node.js](https://nodejs.org/docs/latest/api/)
- [npm](https://docs.npmjs.com/)
- [Mocha](https://mochajs.org/)
- [Supertest](https://github.com/forwardemail/supertest)
- [Chai](https://www.chaijs.com/guide/)
- [dotenv](https://github.com/motdotla/dotenv)
- [Mochawesome](https://github.com/adamgruber/mochawesome)

## Projetos relacionados

- Testes automatizados: [gabrielleqa-santos/banco-api-tests](https://github.com/gabrielleqa-santos/banco-api-tests)
- API utilizada nos testes: [juliodelimas/banco-api](https://github.com/juliodelimas/banco-api)

## Licença

Este projeto está configurado com a licença `ISC` no `package.json`.
