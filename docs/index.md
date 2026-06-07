# Janus Parts Docs

Documentação técnica e operacional do projeto **Janus Parts**.

O Janus Parts é um empresa especializada em vendas de peças automotivas e utiliza um sistema de automação, publicação e controle de vendas através da plataforma do Mercado Livre.

## Objetivo

Automatizar o fluxo de publicação de autopeças no Mercado Livre, reduzindo trabalho manual e padronizando:

- Cadastro de anúncios;
- Imagens dos produtos;
- Preços;
- Estoque;
- Referências;
- Mercado Envios;
- Atualização em lote.

## Fluxo geral

```text
Planilha Excel
↓
Produto
↓
Geração de imagens
↓
Upload para Mercado Livre
↓
Criação ou atualização do anúncio
↓
Gravação do ID ML na planilha