![Título e Imagem de capa](https://i.imgur.com/xG74tOh.png)
# Desafio Módulo 2 - Back-end
## API de um banco digital


![Badges](https://img.shields.io/badge/STATUS-EM%20DESENVOLVIMENTO-GREEN)![Badges](https://img.shields.io/badge/ÚLTIMA%20ATUALIZAÇÃO-SETEMBRO-yellow)

![Badges](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![Badges](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
## Descrição do projeto

Como desenvolvedor recém contratado da melhor empresa de tecnologia do mundo: a CUBOS. Recebi a tarefa de desenvolver uma API para um banco digital, um protótipo que será implementado ao passar do tempo.
## Funcionalidades

Foi construída uma API que tem as seguintes funcionalidades:

-   Criar conta bancária
-   Listar contas bancárias
-   Atualizar os dados do usuário da conta bancária
-   Excluir uma conta bancária
-   Depositar em uma conta bancária
-   Sacar de uma conta bancária
-   Transferir valores entre contas bancárias
-   Consultar saldo da conta bancária
-   Emitir extrato bancário


## Pré-requisitos

![VS Code](https://img.shields.io/badge/VS%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![Insomnia](https://img.shields.io/badge/Insomnia-5849be?style=for-the-badge&logo=Insomnia&logoColor=white)
## Instalação


primeiro deve-se *forkar* o repositório no GitHub.

depois clone o seu repositório para sua máquina através do comando git clone.

```
git clone [endereço]
```

o enreço pode ser obtido seguindo o exemplo da imagem abaixo, primeiro clicando em no botão code (destacado em azul) e em seguida no botão para copiar o endereço (destacado em vermelho).

![imagem](https://i.imgur.com/xcFioNg.png)
<p align="center">  <i><b>FIGURA 1:</b> Obtendo o endereço SSH. </i></p>

instale na pasta raiz do seu projeto a biblioteca express para gerenciar requisições de diferentes verbos HTTP em diferentes URLs, usando o seguinte comando no seu terminal:

```
  npm install express 
```
e para que  servidor seja reiniciado automaticamente com as novas alterações salvas, instale a biblioteca nodemon como dependência de desenvolvimento usando o seguinte comando no seu terminal:

```
npm install -D nodemon
```
além disso precisaremos também ter o insomnia ou alguma outra ferramenta de sua preferência para poder enviar suas solicitações HTTP e também obter as suas respostas
    
## Documentação da API

todas as solicitações usam a porta local 3000 através do endereço `localhost:3000/`

os arquivos da API estão dividido como o padrão REST, nas seguintes pastas.

- src

  - index.js 
  - .gitignore
  - README.md
  - helpers 
    - horario.js
  - database
    - bancodedados.js
  - controllers
    - controladores.js
  - middlewares
    - intermediarios.js
  - routers
    - roteador.js
    

a seguir são mostrados os endpoints e suas funcionalidades.

### Listar todas as contas


esse comando  serve para o gerente ou cargos administrativos do banco terem acesso as contas criadas, bem como seus saldos, senhas e transferências feitas.

```http
  GET /contas
```

| Parâmetro   | Tipo       | Descrição                           |
| :---------- | :--------- | :---------------------------------- |
| `query` | `string` | **Obrigatório**. senha do usuário para ter acesso a essa lista  |

> ***Observação:** Nas demonstrações a seguir iniciamos o aplicativo sem nenhum dado de conta armazenado, assim como seria um banco que não tem clientes.*

#### Retorna

- Exemplo 1: banco de dados vazio
```javascript
  []
```
- Exemplo 2: Listagem com dois usuário criados:
```javascript
[
	{
		"numero": 1,
		"saldo": 0,
		"usuario": {
			"nome": "usuario",
			"cpf": "12312312355",
			"data_nascimento": "2021-03-15",
			"telefone": "71999998888",
			"email": "usuario@banco.com",
			"senha": "12345"
		}
	},
	{
		"numero": 2,
		"saldo": 0,
		"usuario": {
			"nome": "Segundo usuario",
			"cpf": "45645645688",
			"data_nascimento": "2021-03-15",
			"telefone": "71999998888",
			"email": "segundousuario@banco.com",
			"senha": "12345"
		}
	}
]
```

#### Caso a senha informada seja inválida ou esteja em branco a resposta será:

```javascript
{
	"mensagem": "A senha do banco informada é inválida!"
}
```

### Criar uma conta

Esse é o comando para a criação de uma conta para o usuário.

```http
POST /contas
```

| Parâmetro   | Tipo       | Descrição                           |
| :---------- | :--------- | :---------------------------------- |
| `body` | `object` | **Obrigatório**. Objeto com os dados do cliente.  |

Objeto a ser informado no body da requisição deve seguir o seguinte formato.

```javascript
{     
	"nome": "usuario", 
	"cpf": "45645645688",   
	"data_nascimento": "2021-03-15",
	"telefone": "71999998888",  
	"email": "segundousuario@banco.com",   
	"senha": "12345"
}
```
Após requisição ser enviada as seguintes mensagens são exibidas.

> ***Observação:* O número atribuído a conta criada é feito de forma automática, sendo que cada conta tenha uma numeração única e todas as contas são iniciadas com saldo de 0,00.**

### Resposta bem sucedida:

<p align="center">
  <img src="https://i.imgur.com/IG0sv5P.png" alt="Exemplo de resposta bem sucedida." />
</p>
<p align="center">  <i><b>FIGURA 2:</b> Exemplo de resposta bem sucedida. </i></p>




Como mostra a figura 2, o requisição não gera uma resposta ao usuário a não ser o status 201 (created).

### Falhas na requisição:

Foram implementados alguns métodos de validação para possíveis erros que o usuário venha a cometer. A seguir são exemplificados alguns exemplos

- **Parâmetro em branco:**

Caso alguma das propriedades não for informada no body da requisição, a API retornará a seguinte mensagem de erro.

![erro formulário incompleto](https://i.imgur.com/yrEoEsq.png)
<p align="center">  <i><b>FIGURA 3:</b> Resposta para o preenchimento incorreto de algum dado. </i></p>

Observa-se na figura 3 que além da `{"mensagem": "Formulário incompleto!"}` tamém é retornado o status code 400 (bad request).

 - **Cadastrar CPF ou E-mail já existentes em outra conta:**

Cada usuário só pode ter uma conta dentro do banco e isso é analisado através do CPF e e-mail.

![erro cpf ou email](https://i.imgur.com/e7fqiQ2.png)
<p align="center">  <i><b>FIGURA 4:</b> Resposta para CPF ou e-mail já cadastrado em outra conta. </i></p>

A figura 4 mostra que a resposta nesse caso será ``{"mensagem": "Já existe uma conta com o cpf ou e-mail informado!`` junto com o status code 409 (conflict).

### Alterar usuário 

Esse comando permite alterar dados do usuário vinculados a uma determinada conta.

```http
PUT /contas/:numeroConta/usuario
```

| Parâmetro   | Tipo       | Descrição                           |
| :---------- | :--------- | :---------------------------------- |
| `query` | `number` | **Obrigatório**. Número da conta a ter as informações alteradas.  |
| `body` | `object` | **Obrigatório**. Dados que serão substituídos na conta informada.

Essa requisição é enviado no seu body o objeto contendo os dados do usuário a der utilizado, sendo o objeto no seguinte formato:

```javascript
{
	"nome": "usuario Original",
  	"cpf": "12312312355",
  	"data_nascimento": "1995-09-28",
  	"telefone": "84999123456",
  	"email": "usuariooriginal@gmail.com",
	"senha": "12345"
}
```
após o envio da requisição as seguintes mensages podem ser exibidas.

### Resposta bem sucedida

<p align="center">
  <img src="https://i.imgur.com/FLNfQBY.png" alt="Exemplo de resposta bem sucedida." />
</p>
<p align="center">  <i><b>FIGURA 5:</b> Exemplo de alteração bem sucedida. </i></p>

A figura 5 nos mostra que quando a requisição é bem sucedida a API nos retorna apenas o status code 200 (OK).

e na tabela a seguir podes ver o comando `Listar` sendo usado antes e depois da requição de alteração ser enviada.

| Antes   | Depois       | 
| :----------: | :---------: | 
|![antes](https://i.imgur.com/Jv4My33.png) | ![depois](https://i.imgur.com/zVINCsh.png)



### Falhas na requisição:

Nossa API está preparada para tratar também alguns tipo de falhas na requisição.

 - **Parâmetro em branco:**

 Caso o usuário ofereça algum parâmetro ou até mesmo todo o formulário em branco.

<p align="center">
  <img src="https://i.imgur.com/Y5OEccG.png" alt="Exemplo de resposta bem sucedida." />
</p>
<p align="center">  <i><b>FIGURA 6:</b> Exemplo de falha por não informar algum parâmetro da requisição. </i></p>

- **Cadastrar CPF ou E-mail já existentes em outra conta:**

Como já citado, no banco de dados só pode ter uma conta vinculada ao mesmo CPF ou e-mail, A figura 7 nos mostra como é respotado esse erro bem como o seu status code.

<p align="center">
  <img src="https://i.imgur.com/iDgvX1s.png" alt="Erro por já existir o CPF e o e-mail informados viculados a outra conta." />
</p>
<p align="center">  <i><b>FIGURA 7:</b> Erro por já existir o CPF e o e-mail informados viculados a outra conta. </i></p>

- **Buscar uma conta que não existe:**

Tendo em vista que é impossível alterar os dados de uma conta que ainda não existe, a API já foi desenvolvida para nos ajudar quando esse erro ocorrer.

<p align="center">
  <img src="https://i.imgur.com/LqOaeZG.png" alt="Erro por Tentando alterar os dados de uma conta que não existe." />
</p>
<p align="center">  <i><b>FIGURA 8:</b> Tentando alterar os dados de uma conta que não existe. </i></p>

Temos na figura 8 a exemplificação da resposta obtida ao se tentar alterar uma conta que ainda não exite, foi enviado o número 2 no parâmetro do tipo `query`. E como reposta obtivemos ``{"mensagem": "a conta informada não foi encontrada!"}`` e também o status code 404 (Not Found).
