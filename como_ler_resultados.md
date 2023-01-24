# Como ler os resultados

## JSON de erro
Os erros são apresentados em formato JSON, sendo um array, onde cada posição do array reflete uma página e dentro deste objeto (página) existe o array errors que apresentam os erros encontrados na página.
Os atributos mais importantes a serem notados dentro do objeto errors são:

 - instancePath: mostra o atributo onde o problema ocorreu
 - keyword: tipo de problema (ex: pattern (geralmente um regex), maxLength, type, etc...
 - message: mensagem (em inglês) do problema que foi encontrado
 - data: o dado que foi validado e está incorreto

Um exemplo de JSON de erro está abaixo e como interpreta-lo está depois de sua apresentação.

```json
[
  {
    "url": "https://api.algumbanco.com/open-banking/products-services/v1/business-accounts",
    "errors": [
      {
        "instancePath": "/data/brand/companies/0/cnpjNumber",
        "schemaPath": "#/properties/data/properties/brand/properties/companies/items/allOf/0/allOf/0/properties/cnpjNumber/maxLength",
        "keyword": "maxLength",
        "params": {
          "limit": 14
        },
        "message": "must NOT have more than 14 characters",
        "schema": 14,
        "data": "99.999.999/0000-11"
      },
      {
        "instancePath": "/data/brand/companies/0/businessAccounts/0/fees/services/2/maximum/value",
        "schemaPath": "#/properties/data/properties/brand/properties/companies/items/properties/businessAccounts/items/properties/fees/properties/services/items/properties/maximum/properties/value/pattern",
        "keyword": "pattern",
        "params": {
          "pattern": "^((\\d{1,9}\\.\\d{2}){1}|NA)$"
        },
        "message": "must match pattern \"^((\\d{1,9}\\.\\d{2}){1}|NA)$\"",
        "schema": "^((\\d{1,9}\\.\\d{2}){1}|NA)$",
        "data": "4.9"
      }
    ]
  }
]
```

No exemplo acima, o validador ao obter o retorno da URL https://api.algumbanco.com/open-banking/products-services/v1/business-accounts encontrou 2 problemas.
- O primeiro problema está no campo data.brand.companies[0].cnpjNumber, onde o mesmo deveria ter 14 caracteres mas tem 18, pois está sendo retornado com formatação.
- O segundo problema está no campo data.brand.companies[0].businessAccounts[0].fees.services[2].maximum.value que deveria ter duas casas decimais (e não apenas uma como foi apresentado no atributo data "4.9") ou o valor **NA**
