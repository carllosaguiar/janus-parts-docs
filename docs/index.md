# Janus Parts Docs

Documentação técnica e operacional do projeto **Janus Parts**.

O Janus Parts é um sistema interno para automatizar a criação e atualização de anúncios no Mercado Livre a partir de uma planilha de produtos.

## Objetivo

Automatizar o fluxo de publicação de autopeças no Mercado Livre, reduzindo trabalho manual e padronizando:

- cadastro de anúncios;
- imagens dos produtos;
- preços;
- estoque;
- referências;
- Mercado Envios;
- atualização em lote.

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