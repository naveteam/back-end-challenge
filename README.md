# Desafio de back-end

## Navedex API

### O Sistema:

O sistema consiste em um criador de navedex's, nele tu poderá se cadastrar utilizando `email` e `senha`, e então ao logar terá acesso ao banco de dados dos seus navers, possuindo informações como: nomes, data de nascimento, cargos, tempo de empresa e projetos que participou.

Esse Banco de dados será estruturado por você, lembrando que é obrigatório as entidades de `users`, `navers` e `projetos` sendo relacionadas entre si de alguma maneira, deverá ser possível saber em quais projetos um naver está e vice-versa, tudo baseado no usuário que fez a requisição.

### O que deve ser entregue:

Deverá ser implementado uma API [node.js](https://nodejs.org) no padrão [`RESTful`](https://becode.com.br/o-que-e-api-rest-e-restful/#:~:text=REST%20significa%20Representational%20State%20Transfer,abstra%C3%A7%C3%A3o%20da%20arquitetura%20da%20Web.) que possibilite as funcionalidades descritas abaixo:

Junto a API, uma documentação de como testar o sistema é muito bem-vinda. Muitas vezes o README.md do projeto basta, porém também é interessante disponibizar documentação a partir de algum software para testar suas requests, recomendamos [postman](https://www.postman.com/) ou [insomnia](https://insomnia.rest/download/);

Tudo isso deve ser colocado em um repositório público do seu github pessoal, no final a entrega de seu teste será o link para esse repositório.

### Funcionalidades

- Autenticação

    - (Signup) Rota para poder se cadastrar 
        - Deverá receber email e senha e criar novo registro no banco

    - (Login) Rota para poder logar no sistema
        - Deverá retornar um token [JWT](https://jwt.io/) para o usuário ter acesso à outras rotas

OBS: ```AS ROTAS A BAIXO NÃO PODEM SER ACESSADAS CASO O USUÁRIO NÃO POSSUA O TOKEN DE ACESSO (RECEBIDO NO LOGIN)```

- Navers

  - (Index) Rota para listagem dos Navers.

    - Filtrar por nome, tempo de empresa e cargo.
    - Entregará como retorno um vetor com todos os navers ou filtrado por algum dos parâmetro acima, pertencentes ao usuário que fez a request, exemplo:
      ```
          [
              {
                  id: 1,
                  name: Fulano,
                  birthdate: 1999-05-15,
                  admission_date: 2020-06-12,
                  job_role: Desenvolvedor
              },
              {
                  id: 2,
                  name: Ciclano,
                  birthdate: 1992-10-28,
                  admission_date: 2018-06-12,
                  job_role: Desenvolvedor
              }
          ]
      ```

  - (Show) Rota para detalhar informações de um único naver através de seu identificador

    - Além das informações do naver, trazer quais projetos este participou
    - Entregará como retorno um objeto contendo informações sobre o Naver, exemplo:
      ```
      {
          id: 1,
          name: Fulano,
          birthdate: 1999-05-15,
          admission_date: 2020-06-12,
          job_role: Desenvolvedor,
          projects: [
              {
                  id: 3,
                  name: Projeto muito Bom
              }
          ]
      }
      ```

  - (Store) Rota de Criação de Naver
    - Recebe através do body da request os dados do naver e um vetor com os identificadores dos projetos que ele participa e cria um novo registro no banco de dados vinculado ao usuário que fez a request
      ```
          {
              name: Fulano,
              birthdate: 1999-05-15,
              admission_date: 2020-06-12,
              job_role: Desenvolvedor,
              projects: [3]
          }
      ```
    - Entregará como retorno o objeto do usuário criado


  - (Update) Rota Para Atualização de Naver
    - Recebe através do body da request os dados do naver e um vetor com os identificadores dos projetos que ele participa e atualiza seu registro no banco de dados
    - Um usuário só pode editar seus próprios navers
      ```
          {
              name: Fulano,
              birthdate: 1999-05-15,
              admission_date: 2020-06-12,
              job_role: Desenvolvedor,
              projects: [3]
          }
      ```
    - Entregará como retorno o objeto do usuário Atualizado

  - (Delete) Rota Para Deletar um Naver
    - Recebe um identificador de naver e o remove dos registros do banco.
    - Um usuário só pode deletar seus próprios navers.

* Projetos

  - (Index) Rota para listagem dos Projetos
    - Filtrar por nome
    - Entregará como retorno um vetor com todos os projetos ou filtrado pelo nome, pertencentes ao usuário que fez a request, exemplo:
      ```
          [
              {
                  id: 3,
                  name: Projeto muito Bom
              },
              {
                  id: 5,
                  name: Projeto Realmente Bom
              }
          ]
      ```
  - (Show) Rota para detalhar um projeto

    - Além das informações do projeto, trazer quais foram os navers que participaram
    - Entregará como retorno um objeto contendo informações sobre o projeto, exemplo:
      ```
              {
                  id: 3,
                  name: Projeto muito Bom,
                  navers: [
                      {
                          id: 1,
                          name: Fulano,
                          birthdate: 1999-05-15,
                          admission_date: 2020-06-12,
                          job_role: Desenvolvedor
                      }
                  ]
              }
      ```

  - (Store) Rota de Criação de Projeto
    - Recebe através do body da request os dados do projeto e um vetor com os identificadores dos navers que trabalham nele e cria um novo registro no banco de dados vinculado ao usuário que fez a request
      ```
          {
              name: Projeto Bom,
              navers: [1]
          }
      ```
    - Entregará como retorno o objeto do Projeto criado


  - (Update) Rota Para Atualização de Projeto
    - Recebe através do body da request os dados do projeto e um vetor com os identificadores dos navers que trabalham nele e atualiza seu registro no banco de dados
    - Um usuário só pode editar seus próprios projetos
      ```
          {
              name: Projeto Bom,
              navers: [1]
          }
      ```
    - Entregará como retorno o objeto do usuário Atualizado

  - (Delete) Rota Para Deletar um Projeto
    - Recebe um identificador de projeto e o remove dos registros do banco.
    - Um usuário só pode deletar seus próprios projetos

## Observações

As respostas da API devem ser em formato JSON como nos exemplos acima.

Sugestão de bibliotecas para montar a api:

- Koa ou express
- Alguma biblioteca para abstrair a camada de dados que preferir.
  - Knex
  - Bookshelf
  - Objection
  - Mongoose

Prefira o uso de um banco de dados relacional (postgresql, mysql, ...), sendo seu uso não obrigatório.<br>
Para organizar a estrutura de seu projeto prefira o uso do padrão `MVC` sendo seu uso não obrigátório.<br>
Será observado organização de código, legibilidade e melhor uso dos recursos da linguagem javascript.

Se durante o processo de desenvolvimento não conseguiu fazer algo, explique qual o impedimento que encontrou e como tentou resolver em uma seção `Dificuldades` do seu README.md e nos submite até onde chegou :smile:
