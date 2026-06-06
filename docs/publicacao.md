# Publicação de Anúncios

O processo de publicação do Janus Parts é executado através do arquivo:

```text
publish.py
```

Este arquivo é responsável por sincronizar os produtos da planilha com os anúncios existentes no Mercado Livre.

---

## Objetivo

Automatizar a criação e atualização de anúncios a partir da planilha de produtos.

O sistema é capaz de:

- criar anúncios novos;
- atualizar anúncios existentes;
- atualizar imagens;
- atualizar preço;
- atualizar estoque;
- atualizar configuração de envio;
- gravar automaticamente o ID do anúncio na planilha.

---

## Pré-requisitos

Antes de executar a publicação, verificar:

## Planilha

Arquivo:

```text
data/produtos.xlsx
```

Verificar:

- preços atualizados;
- estoque atualizado;
- referências preenchidas;
- fornecedor preenchido.

---

## Imagens

Cada produto deve possuir uma imagem correspondente na pasta:

```text
imagens/
```

Exemplo:

```text
imagens/113320.jpg
imagens/113348.jpg
```

Extensões aceitas:

- jpg
- jpeg
- png
- webp

---

## Credenciais Mercado Livre

Verificar o arquivo:

```text
.env
```

Campos obrigatórios:

```env
ML_CLIENT_ID=
ML_CLIENT_SECRET=
ML_REFRESH_TOKEN=
ML_ACCESS_TOKEN=
```

---

## Executando a Publicação

Ativar o ambiente virtual:

```bash
source .venv/bin/activate
```

Executar:

```bash
python publish.py
```

---

## Fluxo Completo

Para cada produto da planilha, o sistema executa:

```text
Produto
↓
Validação dos dados
↓
Validação da imagem
↓
Geração das imagens finais
↓
Upload das imagens
↓
Criação ou atualização do anúncio
↓
Próximo produto
```

---

## Validações

Antes de publicar, o sistema verifica:

## Referência

A referência é obrigatória.

Exemplo:

```text
CE19395C
```

Caso esteja vazia:

```text
Produto sem referência
```

---

## Preço

O preço deve ser maior que zero.

Exemplo válido:

```text
3627.26
```

Exemplos inválidos:

```text
0
-10
```

---

## Estoque

O estoque deve ser igual ou maior que zero.

Exemplos válidos:

```text
0
5
100
```

---

## Imagem

O sistema verifica se existe imagem para o produto.

Exemplo:

```text
imagens/113320.jpg
```

Se não encontrar:

```text
Imagem não encontrada
```

---

## Geração das Imagens

São geradas duas imagens para cada produto.

## Imagem do Produto

Arquivo:

```text
output/CODIGO_produto.png
```

Características:

- fundo branco;
- produto centralizado;
- sem logotipo;
- sem texto;
- utilizada como imagem principal do anúncio.

---

## Imagem Institucional

Arquivo:

```text
output/CODIGO_institucional.png
```

Características:

- identidade visual Janus Parts;
- utilizada como segunda imagem do anúncio.

---

## Upload das Imagens

Após a geração, as imagens são enviadas para o Mercado Livre.

A API retorna um identificador:

Exemplo:

```text
804960-MLB111656899436_062026
```

Esse identificador é utilizado para montar a galeria do anúncio.

---

## Atualização de Anúncios

Quando o campo `ID ML` estiver preenchido:

Exemplo:

```text
MLB6880703648
```

O sistema atualiza:

- imagens;
- preço;
- estoque;
- Mercado Envios.

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
Atualizar shipping
```

---

## Criação de Anúncios

Quando o campo `ID ML` estiver vazio:

```text
ID ML =
```

O sistema cria um anúncio novo.

Fluxo:

```text
Produto sem ID ML
↓
Criar anúncio
↓
Receber MLBxxxxxxxx
↓
Salvar automaticamente na planilha
```

---

## Mercado Envios

Os anúncios são criados utilizando:

```python
"shipping": {
    "mode": "me2",
    "local_pick_up": False,
    "free_shipping": False
}
```

Benefícios:

- cálculo automático de frete;
- previsão de entrega;
- geração automática de etiqueta;
- rastreamento integrado.

---

## Tratamento de Erros

Quando ocorre erro em um produto:

```text
ERRO 113320
Mensagem do erro...
```

O sistema registra o problema e continua processando os demais produtos.

Isso evita que uma falha interrompa todo o lote.

---

## Resumo Final

Ao final da execução é exibido um resumo:

```text
============================================================
SUCESSOS: 56
CRIADOS: 0
ATUALIZADOS: 56
ERROS: 0
PROCESSAMENTO FINALIZADO
```

## Significado

### SUCESSOS

Quantidade total de produtos processados com sucesso.

### CRIADOS

Quantidade de anúncios novos criados.

### ATUALIZADOS

Quantidade de anúncios existentes atualizados.

### ERROS

Quantidade de produtos que apresentaram falha durante o processamento.

---

## Boas Práticas

Antes de executar:

- salvar a planilha;
- verificar imagens;
- conferir preços;
- conferir estoque.

Após executar:

- verificar o resumo final;
- verificar anúncios atualizados no Mercado Livre;
- verificar possíveis erros reportados.

---

## Frequência Recomendada

Para manter os anúncios sincronizados:

```text
Diariamente
```

ou sempre que houver alteração de:

- preço;
- estoque;
- novos produtos;
- imagens.

---

## Histórico do Projeto

## Primeira Publicação

Primeiro lote publicado com sucesso:

```text
56 produtos
0 erros
Mercado Envios (ME2) ativo
```

Durante o desenvolvimento foram implementadas:

- criação automática de anúncios;
- atualização automática de anúncios;
- gravação automática do ID ML;
- geração de imagens padronizadas;
- suporte ao Mercado Envios;
- atualização automática de preço e estoque.

Este marco representa a entrada do Janus Parts em operação real.