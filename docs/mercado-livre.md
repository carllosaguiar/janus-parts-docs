# Mercado Livre

O Janus Parts utiliza a API oficial do Mercado Livre para criar, atualizar e sincronizar anúncios automaticamente.

Esta integração elimina a necessidade de cadastro manual através do painel do Mercado Livre.

---

## Objetivo

Automatizar:

- criação de anúncios;
- atualização de anúncios existentes;
- atualização de preço;
- atualização de estoque;
- atualização de imagens;
- configuração de Mercado Envios;
- sincronização entre planilha e marketplace.

---

## Aplicação Mercado Livre

A integração utiliza uma aplicação criada no portal de desenvolvedores do Mercado Livre.

Credenciais utilizadas:

```env
ML_CLIENT_ID=
ML_CLIENT_SECRET=
ML_REFRESH_TOKEN=
ML_ACCESS_TOKEN=
```

Estas informações ficam armazenadas no arquivo:

```text
.env
```

---

## Estrutura da Integração

Os serviços relacionados ao Mercado Livre estão localizados em:

```text
src/services/
```

Arquivos:

```text
ml_auth_service.py
ml_image_service.py
ml_item_service.py
```

---

## ml_auth_service.py

Responsável pela autenticação.

Implementação atual:

```python
class MLAuthService:

    def get_access_token(self):
        return os.getenv(
            "ML_ACCESS_TOKEN"
        )
```

Atualmente o sistema utiliza o token armazenado no arquivo `.env`.

---

## ml_image_service.py

Responsável pelo upload das imagens.

Funções:

- envio da imagem;
- obtenção do ID da imagem.

Fluxo:

```text
Imagem local
↓
Upload Mercado Livre
↓
Picture ID
```

Exemplo de retorno:

```text
804960-MLB111656899436_062026
```

Este identificador é utilizado para montar a galeria do anúncio.

---

## ml_item_service.py

Responsável pelo gerenciamento dos anúncios.

Métodos implementados:

```text
get_item()
list_user_items()
create_item()
update_image()
update_price()
update_inventory()
update_shipping()
```

---

## Consulta de Anúncio

Permite consultar um anúncio existente.

Exemplo:

```python
item = service.get_item(
    "MLB6880703648"
)
```

---

## Listagem de Anúncios

Permite listar anúncios da conta.

Exemplo:

```python
itens = service.list_user_items()
```

Resultado:

```python
[
    "MLB6880703648",
    "MLB6902472512"
]
```

---

## Criação de Anúncio

Utilizada quando o produto não possui ID ML.

Fluxo:

```text
Produto sem ID ML
↓
Upload das imagens
↓
Criação do anúncio
↓
Recebe MLBxxxxxxxx
↓
Salva na planilha
```

Campos enviados:

- título;
- categoria;
- preço;
- estoque;
- imagens;
- marca;
- referência;
- tipo de veículo;
- configuração de envio.

---

## Atualização de Anúncio

Utilizada quando o produto já possui ID ML.

Fluxo:

```text
Produto com ID ML
↓
Atualizar imagens
↓
Atualizar preço
↓
Atualizar estoque
↓
Atualizar envio
```

---

## Atualização de Imagens

Substitui completamente as imagens do anúncio.

Exemplo:

```python
service.update_image(
    "MLB6880703648",
    [
        produto_id,
        institucional_id
    ]
)
```

---

## Atualização de Preço

Exemplo:

```python
service.update_price(
    "MLB6880703648",
    3627.26
)
```

---

## Atualização de Estoque

Exemplo:

```python
service.update_inventory(
    "MLB6880703648",
    5
)
```

---

## Mercado Envios

Após a configuração da conta, os anúncios passaram a utilizar:

```python
"shipping": {
    "mode": "me2",
    "local_pick_up": False,
    "free_shipping": False
}
```

Benefícios:

- cálculo automático do frete;
- previsão de entrega;
- geração automática de etiquetas;
- rastreamento integrado.

---

## Fluxo de Venda

Quando um comprador realiza uma compra:

```text
Compra
↓
Pagamento aprovado
↓
Etiqueta gerada pelo Mercado Livre
↓
Vendedor imprime a etiqueta
↓
Postagem
↓
Rastreamento automático
↓
Entrega
↓
Liberação do pagamento
```

---

# Tratamento de Erros

## Token inválido

Exemplo:

```json
{
  "code": "unauthorized",
  "message": "invalid access token"
}
```

Solução:

Renovar o access token.

Consultar:

```text
autenticacao.md
```

---

## ID inválido

Exemplo:

```text
item.id.invalid
```

Possíveis causas:

- ID ML vazio;
- ID ML incorreto;
- anúncio removido.

---

## Imagem rejeitada

O Mercado Livre pode rejeitar imagens que contenham:

- logotipos;
- textos promocionais;
- marcas d'água.

Por este motivo o Janus Parts utiliza:

```text
Imagem 1 → Produto limpo
Imagem 2 → Institucional Janus
```

---

## Histórico do Projeto

## Primeira Integração Operacional

Resultados obtidos:

```text
56 anúncios publicados
0 erros
Mercado Envios ativo
Atualização automática funcionando
```

O projeto passou a operar em ambiente real utilizando a API oficial do Mercado Livre.

---

## Melhorias Futuras

## Renovação Automática de Token

Implementar:

```text
refresh token automático
```

---

## Descrições Automáticas

Gerar descrições a partir dos dados da planilha.

---

## Especificações Técnicas

Reduzir avisos de:

```text
incomplete_technical_specs
```

através do preenchimento automático de atributos adicionais.

---

## Sincronização Bidirecional

Possibilidade futura de:

```text
Mercado Livre
↓
Janus Parts
↓
Planilha
```

permitindo atualizar a planilha a partir dos anúncios existentes.