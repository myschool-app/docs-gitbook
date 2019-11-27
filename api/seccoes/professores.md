---
description: API dos Professores
---

# Professores

{% api-method method="get" host="https://myschool-app.com/api" path="/professores" %}
{% api-method-summary %}
Lista de Professores
{% endapi-method-summary %}

{% api-method-description %}
Este _endpoint_ permite obter uma lista de todos os professores registados na aplicação.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="NumObjetos" type="integer" required=false %}
Número de objetos retornados pela API.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Lista retornada com sucesso.
{% endapi-method-response-example-description %}

```
{"id": 3, "utilizador_id": 1, "nome": "Alexandre Faria", "email": "", "telefone": ""}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



