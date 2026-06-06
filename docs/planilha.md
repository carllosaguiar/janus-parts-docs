# Planilha de Produtos

O Janus Parts utiliza uma planilha Excel como fonte principal de dados para criação e atualização dos anúncios no Mercado Livre.

Arquivo padrão:

```text
data/produtos.xlsx
```

---

## Estrutura da Planilha

A primeira linha da planilha deve conter exatamente os seguintes cabeçalhos:

| Coluna | Obrigatória | Descrição |
|----------|----------|----------|
| UN | Sim | Unidade do produto |
| CODIGO MERC. | Sim | Código interno do produto |
| DESCRICAO MERCADORIA | Sim | Descrição principal utilizada no anúncio |
| PREÇO | Sim | Preço de venda |
| ESTOQUE TOTAL | Sim | Quantidade disponível |
| REFERENCIA | Sim | Número de referência da peça |
| APLICACAO | Não | Aplicação do produto |
| COD. BARRAS | Não | Código de barras |
| FORNECEDOR | Sim | Marca ou fabricante |
| ID ML | Não | Código do anúncio no Mercado Livre |

---

## Exemplo

| CODIGO MERC. | DESCRICAO MERCADORIA | PREÇO | ESTOQUE TOTAL | REFERENCIA | FORNECEDOR | ID ML |
|----------|----------|----------|----------|----------|----------|----------|
| 113320 | HELICE MOTOR VISCOSA VOLVO NL10 | 3627.26 | 5 | CE19395C | CEMAK | MLB6880703648 |

---

## Funcionamento do Campo ID ML

O campo `ID ML` controla se o sistema irá criar ou atualizar um anúncio.

## Quando está vazio

Exemplo:

```text
ID ML =
```

O sistema entende que o produto ainda não foi publicado.

Fluxo:

```text
Planilha
↓
Produto sem ID ML
↓
Cria anúncio
↓
Recebe MLBxxxxxxxx
↓
Salva automaticamente na planilha
```

---

## Quando está preenchido

Exemplo:

```text
ID ML = MLB6880703648
```

O sistema entende que o anúncio já existe.

Fluxo:

```text
Planilha
↓
Produto com ID ML
↓
Atualiza anúncio existente
```

Serão atualizados:

- imagens;
- preço;
- estoque;
- configuração de envio.

---

## Nome das Imagens

Para cada produto deve existir uma imagem correspondente na pasta:

```text
imagens/
```

O nome do arquivo deve ser igual ao código interno do produto.

Exemplos:

```text
imagens/113320.jpg
imagens/113348.jpg
imagens/113410.png
```

Extensões aceitas:

- jpg
- jpeg
- png
- webp

---

## Regras de Preenchimento

## Código do Produto

Deve ser único.

Exemplo:

```text
113320
```

Não devem existir duas linhas com o mesmo código.

---

## Preço

Deve ser maior que zero.

Exemplo:

```text
3627.26
```

Valores inválidos:

```text
0
-10
```

---

## Estoque

Deve ser igual ou maior que zero.

Exemplos válidos:

```text
0
1
15
150
```

---

## Referência

Campo obrigatório.

Exemplo:

```text
CE19395C
```

Utilizado para preencher atributos do anúncio.

---

## Fornecedor

Campo obrigatório.

Exemplo:

```text
CEMAK
```

Utilizado para preencher a marca do anúncio.

---

## Processo de Publicação

A publicação é realizada através do comando:

```bash
python publish.py
```

O sistema executa automaticamente:

1. leitura da planilha;
2. validação dos dados;
3. validação da imagem;
4. geração das imagens do anúncio;
5. upload das imagens;
6. criação ou atualização do anúncio;
7. gravação automática do ID ML quando necessário.

---

## Resultado Esperado

Ao final da execução o sistema apresenta um resumo:

```text
============================================================
SUCESSOS: 56
CRIADOS: 0
ATUALIZADOS: 56
ERROS: 0
PROCESSAMENTO FINALIZADO
```

---

## Boas Práticas

## Antes de executar

Verificar:

- se a planilha foi salva;
- se todos os produtos possuem imagem;
- se os preços estão atualizados;
- se os estoques estão corretos.

## Após executar

Verificar:

- quantidade de sucessos;
- quantidade de erros;
- anúncios atualizados no Mercado Livre.

---

## Observações

O campo `ID ML` nunca deve ser editado manualmente após a criação do anúncio.

O próprio Janus Parts é responsável por preencher e manter esse campo atualizado.

Alterar manualmente esse valor pode fazer com que o sistema atualize o anúncio errado.