# **_Kenzie Recipes_**

Este é um teste de backend com intuito de aprender mais sobre API's e json-server. Kenzie Recipes têm como objetivo ser um blog de receitas, tendo receitas oficiais com visão à todo público e podendo estar logado para postar as suas receitas! Quem sabe até chegar nas oficiais!

## **Endpoints**

Essa API tem um total de 10 endpoits, sendo eles 3 para cadastro, 2 para login, 1 para listar os dados do usuário logado, 3 para os posts e 2 para as recipes. Url base da API é "https://kenzie-recipes.herokuapp.com".

### Cadastro

POST /register <br/>
POST /signup <br/>
POST /users

Qualquer um desses 3 endpoints é para cadastro de usuário ficando na lista "users", os campos obrigatórios são e-mail e password, podendo passar mais dados caso queira. Exemplo:

````
    {
	"email": "analaura@mail.com",
	"password": "123456"
    }
 ````

201 Created - Se tudo ocorrer corretamente, a resposta será a seguinte: 

````
    {
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImFuYWxhdXJhQG1haWwuY29tIiwiaWF0IjoxNjUxNzczOTA4LCJleHAiOjE2NTE3Nzc1MDgsInN1YiI6IjMifQ.zhVbYHNz1-kSCcFgK1FAw3k9ahKCc_LZtDW0DEfsKbE",
	"user": {
		"email": "analaura@mail.com",
		"id": 3
	    }
    }   
 ````

 400 Bad Request - Resposta caso algum dos dados não seja passado, e-mail já cadastrado ou algum outro erro. 


### Login

POST /login <br/>
POST /signin

Qualquer um desses endpoints acima irá realizar o login de algum usuário já cadastrado em "users". É preciso colocar seu e-mail e password. 

````
    {
	"email": "analaura@mail.com",
	"password": "123456"
    }
 ````

 Possíveis respostas:

 200 OK
 ````
    {
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImFuYWxhdXJhMUBtYWlsLmNvbSIsImlhdCI6MTY1MTc3NDc5NywiZXhwIjoxNjUxNzc4Mzk3LCJzdWIiOiI0In0.4k024V2dzeJ1yRr_G7UFiteXTCkygxltbGcBF_SvoIM",
	"user": {
		"email": "analaura1@mail.com",
		"id": 4
	    }
    }
 ````
 400 Bad Request - Caso algum dado seja passado errado ou ocorra algum outro erro.

 
 ### Users

 GET /users/:id

 Com este endpoint você consegue ver seus dados que estão cadastrados. Somente o dono deste cadastro pode ver os dados, ou seja, caso você queira ver dados de outros usários retornará um erro, você só pode ver seus dados. Você precisará do seu token header para conseguir acessar os dados.

 ````
    {
        "Authorization": "Bearer {token}"
    }
 ````

 Possíveis respostas:

200 OK - Passando o id corretamente do usário logado.
 ````
    {
	"email": "aninha@mail.com",
	"password": "$2a$10$5jY8bu/zpPFIG4m9oWamg.It6.Ep8qMKG4zgsLcsKqRIj.qHc5EbK",
	"age": 23,
	"id": 2
    }
 ````

401 Forbidden - Passando o id incorreto na rota.
````
    "Private resource access: entity must have a reference to the owner id"
````

### Posts

#### Create post

POST /posts

Para criar um post o usuário precisa estar logado, ou seja, ele precisará do token na requisição do header e o id do usuário para poder postar.
 ````
    {
        "post": "Minha receita fresquinha!",
        "userId": 2
    }
 ````

 Possíveis respostas:

201 Created
````
    {
	"post": "Minha receita fresquinha",
	"userId": 2,
	"id": 2
    }
````


 403 Forbidden 
 ````
    "Private resource creation: request body must have a reference to the owner id"
 ````   

#### Read posts

 GET /posts

 Para ler os posts você precisa estar logado, ou seja, você precisa do seu token no header da requisição.

 ````
    "Authorization": "Bearer {token}"
 ````   

 Possíveis respostas:

 200 OK 
 ````
    [
	{
		"post": "Quero conseguir ter minha receita oficial!",
		"userId": 2,
		"id": 1
	},
	{
		"post": "A receita que quero como oficial aqui: ",
		"userId": 1,
		"id": 1
	},
	{
		"post": "Minha receita fresquinha",
		"userId": 2,
		"id": 2
	}
]
 ````

 401 Unauthorized
 ````
    "Missing authorization header"
 ````

 ### Recipes

 GET /recipes

 Com este endpoint você consegue ver todas as receitas oficiais do blog, elas são públicas, logo todos podem vê-las.

 ````
    [
	{
		"name": "Bolo de Chocolate",
		"ingredients": "4 ovos, 4 colheres (sopa) de chocolate em pó, 2 colheres (sopa) de manteiga, 3 xícaras (chá) de farinha de trigo, 2 xícaras (chá) de açúcar, 2 colheres (sopa) de fermento, 1 xícara (chá) de leite",
		"id": 1
	}
]
 ````