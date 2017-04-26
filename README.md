# MongoDB

## Pré-requisitos

- MongoDB: [Link para instalação](https://www.monogodb.org)

## Comandos


### db.createCollection()
- Cria uma coleção de dados (como se fosse a tabela)
```js
db.createCollection('alunos')
```

### db.alunos.insert()
- Insere na coleção
```js
db.alunos.insert(
    {
        "nome" : "Felipe", 
        "data_nascimento" : new Date (1994,02,26)
    }
)
```

