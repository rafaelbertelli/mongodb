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

### db.alunos.find()
- Retorna toda a coleção de dados

```js
db.alunos.find()
```
### db.aluno.remove()
- Remove item da coleção passando um parâmetro de seleção, como o _id, por exemplo.

```js
db.aluno.remove({
    "_id" : ObjectId ("56cb0002b6d75ec12f75d3b5")
})
```

