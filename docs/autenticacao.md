# Autenticação Mercado Livre

O Janus Parts utiliza a API do Mercado Livre para criar e atualizar anúncios.

Para que a integração funcione, é necessário manter um `access_token` válido.

## Credenciais utilizadas

As credenciais ficam armazenadas no arquivo `.env`.

Exemplo:

```env
ML_CLIENT_ID=xxxxxxxx
ML_CLIENT_SECRET=xxxxxxxx
ML_REFRESH_TOKEN=xxxxxxxx
ML_ACCESS_TOKEN=xxxxxxxx
```

## O que é cada campo?

### ML_CLIENT_ID

Identificador da aplicação criada no Mercado Livre.

### ML_CLIENT_SECRET

Chave secreta da aplicação.

### ML_REFRESH_TOKEN

Token de longa duração utilizado para gerar novos access tokens.

### ML_ACCESS_TOKEN

Token utilizado pelo sistema para autenticar chamadas na API.

É este token que expira periodicamente.

---

# Como identificar que o token expirou

Os sintomas mais comuns são erros como:

```json
{
  "code": "unauthorized",
  "message": "invalid access token"
}
```

ou

```text
401 Unauthorized
```

durante:

- criação de anúncios;
- atualização de estoque;
- atualização de preço;
- upload de imagens.

---

# Renovando o Access Token

Quando o token expirar, execute a seguinte requisição.

## Endpoint

```text
POST https://api.mercadolibre.com/oauth/token
```

## Payload

```json
{
  "grant_type": "refresh_token",
  "client_id": "SEU_CLIENT_ID",
  "client_secret": "SEU_CLIENT_SECRET",
  "refresh_token": "SEU_REFRESH_TOKEN"
}
```

## Exemplo usando curl

```bash
curl -X POST https://api.mercadolibre.com/oauth/token \
-H "accept: application/json" \
-H "content-type: application/json" \
-d '{
  "grant_type":"refresh_token",
  "client_id":"SEU_CLIENT_ID",
  "client_secret":"SEU_CLIENT_SECRET",
  "refresh_token":"SEU_REFRESH_TOKEN"
}'
```

---

# Resposta esperada

A API retornará algo semelhante a:

```json
{
  "access_token": "APP_USR-xxxxxxxx",
  "token_type": "bearer",
  "expires_in": 21600,
  "scope": "offline_access read write",
  "user_id": 123456789,
  "refresh_token": "TG-xxxxxxxx"
}
```

---

# Atualizando o arquivo .env

Copie os novos valores retornados.

Atualize:

```env
ML_ACCESS_TOKEN=NOVO_ACCESS_TOKEN
ML_REFRESH_TOKEN=NOVO_REFRESH_TOKEN
```

Importante:

O Mercado Livre normalmente retorna um novo refresh token.

Sempre substitua ambos.

---

# Reiniciando o sistema

Após atualizar o `.env`, execute novamente:

```bash
python publish.py
```

---

# Testando o token

Para validar o token atual:

```python
import requests

from src.config import (
    ML_ACCESS_TOKEN
)

response = requests.get(
    "https://api.mercadolibre.com/users/me",
    headers={
        "Authorization":
        f"Bearer {ML_ACCESS_TOKEN}"
    }
)

print(
    response.status_code
)

print(
    response.json()
)
```

Resultado esperado:

```text
200
```

---

# Melhorias futuras

Atualmente a renovação do token é manual.

Está previsto para uma versão futura do Janus Parts:

- renovação automática do token;
- atualização automática do `.env`;
- tratamento automático de erros 401.