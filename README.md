# MongoDB

## Pré-requisitos

- MongoDB: [Link para instalação](https://www.monogodb.org)

## Comandos

### db
- Mostra db ativo
```js
db
```

### use db_finance
- Acessa db mesmo que não exista fisicamente
```js
use db_finance
```

### db.createCollection()
- Cria uma coleção de dados (como se fosse a tabela)
```js
db.createCollection('alunos')
```

### db.alunos.drop()
- Deleta a collection
    - Se droppar todas as collections, automaticamente o db será droppado
```js
db.alunos.drop()
```

### db.billingCycles.dropDatabase()
- Deleta a Database
```js
db.billingCycles.dropDatabase()
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

### db.alunos.save()
- Insere/Atualiza na coleção *desde que passe uma chave
```js
db.alunos.save(
    {
        "nome" : "Felipe", 
        "data_nascimento" : new Date (1994,02,26)
    }
)
```

### db.alunos.find().pretty()
- Retorna toda a coleção de dados.
Obs: O comando `.pretty()` traz os dados de forma organizada
```js
db.alunos.find().pretty()
```
```js
// find dentro de um array
db.alunos.find({
    "habilidades.nome":"Mochabustão"
}).pretty()
```
```js
// find $or
db.alunos.find({
    $or : [
        {"curso.nome":"Sistemas de informação"},
        {"curso.nome":"MongoDB"}
    ]
}).pretty()
```
```js
// find $or + and (,)
db.alunos.find({
    $or : [
        {"curso.nome":"Sistemas de informação"},
        {"curso.nome":"MongoDB"},
        {"curso.nome":"Quiropraxia"}
    ],
    "nome":"Monaliza"
}).pretty()
```
```js
// find $or + and (,) + $in
db.alunos.find({
    $or : [
        {"curso.nome": {$in : [
            "Sistemas de informação", 
            "MongoDB", 
            "Quiropraxia"
        ]}},
    ],
    "nome":"Monaliza"
}).pretty()
```
```js
// find $in
db.alunos.find({
    'curso.nome' : { $in : [
        'Sistemas de informação', 
        'Quiropraxia'
    ]}
}).pretty()
```
```js
// find dos elementos que existem em credits e retornando somente o name. o _id deve ser passado como zero para nao vir o ID do atributo.
db.billingCycles.find(
    {credits: {$exists: true}},
    {_id:0, name:1}
).pretty()
```

### db.billingCycles.find({credits:{$exists:false}}).pretty()
- Retorna somente os elementos que existem com este valor, ou não.
Obs: O comando `.pretty()` traz os dados de forma organizada
```js
db.billingCycles.find({credits:{$exists:false}}).pretty()
```

### db.alunos.remove()
- Remove item da coleção passando um parâmetro de seleção, como o _id, por exemplo.
```js
db.alunos.remove({
    "_id" : ObjectId ("56cb0002b6d75ec12f75d3b5")
})
```

### db.alunos.update()
- Atualiza registro. 
```js
// o update, por padrão troca somente o primeiro registro encontrado
db.alunos.update(
    {'curso.nome':'Sistemas da Informação'},
    {
        $set : {
            'curso.nome':'Sistemas de Informação'
        }
    }
)
```
```js
// para multiplas linhas, usar o obj { multi : true }
db.alunos.update(
    {'curso.nome':'Sistemas de Informação'},
    {
        $set : {
            'curso.nome':'Sistemas De Informação'
        }
    }, {
        multi : true
    }
)
```
```js
// atualizar array com push de somente um item
db.alunos.update(
    {'nome':'Felipe'},
    {
        $push : {
            'notas' : 8.5
        }
    }
)
```
```js
// atualizar array com push de vários itens
db.alunos.update(
    {'nome':'Felipe'},
    {
        $push : {
            'notas' : { $each : [8.5, 9.5] }
        }
    }
)
```
```js
db.alunos.update(
    {'nome':'Fernando'},
    { 
        $set : {
            'localização' : {
                'endereço':'Rua Eponina Estrela, 68',
                'cidade':'Mauá',
                'coordinates':[-23.680127, -46.413804],
                'type':'Point'
            }
        }
    }
)
```
```js
db.billingCycles.update(
    {
        $and: [ {month:1}, {year:2017} ]
    },
    {
        $set: { credits: [ {name: "Salário", value:3500} ] }
    }
)
```

### db.billingCycles.count()
- Conta os registros
```js
db.billingCycles.count()
```

### db.alunos.find().sort()
- Ordena registros. 
```js
db.alunos.find().sort({'nome': 1}) // crescente
db.alunos.find().sort({'nome': -1}) // decrescente
```

### db.alunos.find().sort().limit()
- Ordena registros com limite de dados.
```js
db.alunos.find().sort({'nome': 1}).limit(2) // TOP 3
```

### db.billingCycles.find().skip(1)
- Pula o primeiro registro
```js
db.billingCycles.find().skip(1)
```

### mongoimport --collection alunos --jsonArray alunos.json
- Importar dados JSON para a collection
```js
// executar na cli, fora do mongo shell
mongoimport --collection alunos --jsonArray alunos.json
```

### db.alunos.createIndex()
- Criação de índice
```js
db.alunos.createIndex({
    localizacao : '2dsphere'
})
```

### db.billingCycles.aggregate()
- Faz agregação de vários items de uma pipeline.
Neste caso, usarei os comandos '$project e '$group, que fazem o SELECT somente dos campos especificados e consolida os dados em um único registro, respectivamente.
```js
db.billingCycles.aggregate([{
    $project: {credito: {$sum: "$credits.value"}, debito: {$sum: "$debts.value"}},
    }, {
    $group: {'_id': null, credito: {$sum: "$credito"}, debito: {$sum: "$debito"}}
}])
```

### db.alunos.aggregate()
- Geolocalização
```js
db.alunos.aggregate([
    {
        $geoNear : {
            near : {
                coordinates : [-23.632169,-46.510285],
                type : 'Point'
            },
            distanceField : 'distancia.calculada',
            spherical : true,
            num : 5
        }
    },
    { 
        $skip : 1
    }
])
```























### 
```js

```

