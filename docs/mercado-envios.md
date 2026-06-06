# Mercado Envios (ME2)

O Mercado Envios 2 (ME2) é o sistema logístico do Mercado Livre responsável pelo cálculo de frete, geração de etiquetas, rastreamento e previsão de entrega dos pedidos.

Após a configuração correta da conta, os anúncios do Janus Parts passaram a utilizar automaticamente o Mercado Envios.

---

## Objetivo

Automatizar todo o processo logístico da venda.

Benefícios:

- cálculo automático do frete;
- previsão de entrega ao comprador;
- geração automática de etiquetas;
- rastreamento integrado;
- atualização automática do status da entrega;
- maior confiança para o comprador.

---

## Situação Inicial

Durante os primeiros testes os anúncios exibiam:

```text
Entrega a combinar com o vendedor
```

Isso ocorria porque os anúncios estavam sendo criados com:

```json
{
  "mode": "not_specified"
}
```

ou

```json
{
  "mode": "custom"
}
```

Nesses modos o Mercado Livre não controla o envio.

---

## Ativação do Mercado Envios

A ativação foi realizada diretamente na conta Mercado Livre.

Após a configuração, a API passou a retornar:

```json
{
  "modes": [
    "custom",
    "not_specified",
    "me2"
  ]
}
```

E também:

```json
{
  "mode": "me2",
  "type": "drop_off",
  "status": "active"
}
```

Isso confirmou que o Mercado Envios estava habilitado para a conta.

---

## Configuração Utilizada

Os anúncios são criados utilizando:

```python
"shipping": {
    "mode": "me2",
    "local_pick_up": False,
    "free_shipping": False
}
```

---

## Tipo de Logística

A conta atualmente utiliza:

```text
drop_off
```

Significado:

```text
Vendedor leva o pacote até o ponto de postagem.
```

Não existe coleta automática no endereço do vendedor.

---

## Fluxo de Compra

## 1. Compra

O comprador realiza a compra do produto.

```text
Compra realizada
```

---

## 2. Aprovação do Pagamento

O Mercado Livre processa o pagamento.

Status comum:

```text
Pagamento aprovado
```

---

## 3. Geração da Etiqueta

Após a aprovação do pagamento, o Mercado Livre gera automaticamente:

- etiqueta;
- código de rastreio;
- dados do destinatário.

O vendedor não precisa preencher endereço manualmente.

---

## 4. Impressão

O vendedor imprime a etiqueta fornecida pelo Mercado Livre.

Formato atual:

```text
PDF A4
```

---

## 5. Embalagem

O produto deve ser embalado adequadamente.

Recomendações:

- proteger contra impactos;
- evitar folgas excessivas;
- utilizar embalagem compatível com o tamanho da peça.

---

## 6. Postagem

O vendedor leva o pacote até o ponto de postagem indicado pelo Mercado Livre.

Fluxo:

```text
Imprimir etiqueta
↓
Embalar produto
↓
Levar ao ponto de postagem
```

---

## 7. Transporte

Após a postagem:

```text
Mercado Livre
↓
Transportadora
↓
Comprador
```

Todo o rastreamento é atualizado automaticamente.

---

## 8. Entrega

Quando o comprador recebe o produto:

```text
Pedido entregue
```

---

## 9. Liberação do Pagamento

Após a confirmação da entrega, o Mercado Livre libera o valor da venda conforme as regras da conta.

Fluxo simplificado:

```text
Compra
↓
Entrega
↓
Prazo de segurança
↓
Liberação do pagamento
```

---

## Atualização dos Anúncios Existentes

Foi implementado no projeto o método:

```python
atualizar_shipping()
```

Responsável por converter anúncios antigos para utilizar ME2.

Exemplo:

```python
item_service.atualizar_shipping(
    id_ml
)
```

---

## Resultado Obtido

Antes:

```text
Entrega a combinar com o vendedor
```

Depois:

```text
Chegará entre dia X e dia Y
```

ou

```text
Chegará grátis a partir de sexta-feira
```

Isso confirma que o Mercado Envios está funcionando corretamente.

---

## Vantagens Operacionais

## Para o Comprador

- previsão de entrega;
- rastreamento;
- maior confiança na compra.

## Para o Vendedor

- geração automática de etiquetas;
- cálculo automático do frete;
- menos trabalho operacional;
- maior conversão dos anúncios.

---

## Problemas Comuns

## Anúncio mostra "Entrega a combinar com o vendedor"

Possíveis causas:

- anúncio criado sem ME2;
- conta sem Mercado Envios habilitado;
- shipping não atualizado.

Solução:

Executar:

```python
item_service.atualizar_shipping(
    id_ml
)
```

ou republicar o anúncio.

---

## API não retorna "me2"

Exemplo:

```json
{
  "modes": [
    "custom",
    "not_specified"
  ]
}
```

Isso significa que o Mercado Envios ainda não foi habilitado para a conta.

---

## Histórico

Durante a primeira publicação operacional do Janus Parts:

```text
56 anúncios atualizados
Mercado Envios habilitado
ME2 funcionando
Previsão de entrega ativa
```

A partir deste momento todos os anúncios passaram a utilizar a logística oficial do Mercado Livre.