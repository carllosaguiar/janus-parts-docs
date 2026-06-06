# Arquitetura

A arquitetura do Janus Parts é organizada por responsabilidade.

## Estrutura de diretórios

```text
janus-parts/
├── assets/
│   ├── templates/
│   │   ├── principal.png
│   │   └── institucional.png
│   ├── logo_janus.jpg
│   └── selo_janus.png
├── data/
│   └── produtos.xlsx
├── imagens/
│   ├── 113320.jpg
│   └── ...
├── output/
│   ├── 113320_produto.png
│   ├── 113320_institucional.png
│   └── ...
├── src/
│   ├── config.py
│   ├── models/
│   │   └── produto.py
│   └── services/
│       ├── excel_service.py
│       ├── image_service.py
│       ├── ml_auth_service.py
│       ├── ml_image_service.py
│       └── ml_item_service.py
├── publish.py
├── requirements.txt
├── README.md
└── .env
```

---

## Responsabilidades

### `publish.py`

Arquivo principal do sistema.

Responsável por orquestrar todo o fluxo:

```text
Planilha
↓
Produto
↓
Geração de imagens
↓
Upload para Mercado Livre
↓
Criação ou atualização do anúncio
```

---

### `src/config.py`

Centraliza configurações do projeto, como:

- caminhos;
- templates;
- fontes;
- credenciais do Mercado Livre;
- margem de preço.

---

### `src/models/produto.py`

Define o modelo `Produto`.

Esse modelo representa uma linha da planilha.

Campos principais:

- código interno;
- descrição;
- preço;
- estoque;
- referência;
- aplicação;
- código de barras;
- fornecedor;
- ID ML.

---

### `src/services/excel_service.py`

Responsável por:

- ler a planilha;
- transformar linhas em objetos `Produto`;
- salvar automaticamente o `ID ML` após criação de anúncios.

---

### `src/services/image_service.py`

Responsável por gerar as imagens utilizadas nos anúncios.

Gera:

```text
output/CODIGO_produto.png
output/CODIGO_institucional.png
```

---

### `src/services/ml_image_service.py`

Responsável pelo upload das imagens para o Mercado Livre.

Recebe uma imagem local e retorna o `picture_id`.

---

### `src/services/ml_item_service.py`

Responsável pela comunicação com a API de anúncios do Mercado Livre.

Métodos principais:

- `obter_item()`
- `listar_itens_usuario()`
- `criar_item()`
- `atualizar_imagens()`
- `atualizar_preco()`
- `atualizar_estoque()`
- `atualizar_shipping()`

---

### `src/services/ml_auth_service.py`

Responsável por obter o token de acesso configurado no ambiente.

Atualmente lê o valor de:

```text
ML_ACCESS_TOKEN
```

---

## Fluxo operacional

```text
data/produtos.xlsx
↓
excel_service.py
↓
Produto
↓
image_service.py
↓
ml_image_service.py
↓
ml_item_service.py
↓
Mercado Livre
```

---

## Regra principal

O campo `ID ML` define o comportamento do sistema.

### Produto sem ID ML

```text
Cria novo anúncio
↓
Recebe MLB...
↓
Salva ID ML na planilha
```

### Produto com ID ML

```text
Atualiza anúncio existente
```
