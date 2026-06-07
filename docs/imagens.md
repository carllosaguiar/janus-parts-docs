# Sistema de Imagens

O módulo de imagens do Janus Parts é responsável por gerar as imagens utilizadas nos anúncios do Mercado Livre.

As imagens são processadas automaticamente durante a execução do:

```bash
python publish.py
```

---

## Objetivo

Padronizar a apresentação visual dos anúncios.

Benefícios:

- identidade visual consistente;
- conformidade com as regras do Mercado Livre;
- redução de trabalho manual;
- atualização automática das imagens dos anúncios.

---

## Estrutura de Diretórios

### Imagens Originais

As imagens fornecidas pela empresa ficam em:

```text
imagens/
```

Exemplos:

```text
imagens/113320.jpg
imagens/113348.jpg
imagens/113410.png
```

O nome do arquivo deve ser igual ao código do produto.

---

## Imagens Geradas

As imagens processadas são armazenadas em:

```text
output/
```

Exemplo:

```text
output/113320_produto.png
output/113320_institucional.png
```

---

## Tipos de Imagem

O sistema gera duas imagens para cada produto.

---

## Imagem do Produto

Arquivo:

```text
output/CODIGO_produto.png
```

Exemplo:

```text
output/113320_produto.png
```

Esta é a imagem principal do anúncio.

---

## Características

- fundo branco;
- produto centralizado;
- formato quadrado;
- sem textos;
- sem logotipos;
- sem marca d'água.

---

## Motivo

O Mercado Livre possui regras rígidas para imagens principais.

A imagem de capa não pode conter:

- logotipos;
- telefones;
- URLs;
- textos promocionais;
- marcas d'água;
- informações comerciais.

Caso contrário o anúncio pode ser:

```text
Pausado
```

ou

```text
Rebaixado nos resultados
```

---

## Dimensões

Canvas utilizado:

```text
1200 x 1200
```

O produto é redimensionado proporcionalmente e centralizado.

---

## Imagem Institucional

Arquivo:

```text
output/CODIGO_institucional.png
```

Exemplo:

```text
output/113320_institucional.png
```

Esta imagem é utilizada como segunda imagem do anúncio.

---

## Objetivo

Apresentar a identidade visual da Janus Parts.

Elementos presentes:

- logotipo Janus Parts;
- identidade visual da empresa;
- informações institucionais.

---

## Utilização

Ordem utilizada nos anúncios:

```text
Imagem 1 → Produto
Imagem 2 → Institucional
```

Esta configuração segue as recomendações do Mercado Livre.

---

## Fluxo de Geração

Durante a execução do publish:

```text
Produto
↓
Localizar imagem original
↓
Gerar imagem do produto
↓
Gerar imagem institucional
↓
Salvar em output/
↓
Enviar para Mercado Livre
```

---

## Formatos Aceitos

O sistema procura automaticamente pelas extensões:

```text
jpg
jpeg
png
webp
```

Exemplo:

```text
113320.jpg
113320.jpeg
113320.png
113320.webp
```

A primeira encontrada será utilizada.

---

## Localização Automática

Implementação:

```python
_localizar_imagem()
```

Fluxo:

```text
Código do produto
↓
Pesquisar extensões conhecidas
↓
Retornar caminho encontrado
```

---

## Erro de Imagem Não Encontrada

Caso nenhuma imagem seja localizada:

```text
Imagem não encontrada
```

O produto será ignorado.

Os demais produtos continuarão sendo processados.

---

## Upload para Mercado Livre

Após a geração, as imagens são enviadas para o Mercado Livre.

Fluxo:

```text
Imagem local
↓
Upload
↓
Picture ID
```

Exemplo:

```text
804960-MLB111656899436_062026
```

Esse identificador é utilizado na criação ou atualização do anúncio.

---

## Atualização das Imagens

Quando um produto já possui ID ML:

```text
MLB6880703648
```

As imagens antigas são substituídas automaticamente.

Fluxo:

```text
Gerar novas imagens
↓
Upload
↓
Atualizar anúncio
```

---

## Boas Práticas

## Utilizar imagens nítidas

Evitar:

- imagens desfocadas;
- imagens com baixa resolução;
- imagens cortadas.

---

## Fundo Original

Sempre que possível utilizar imagens com fundo neutro.

O sistema realizará apenas o enquadramento e centralização.

---

## Nome dos Arquivos

Manter exatamente o código do produto.

Exemplo correto:

```text
113320.jpg
```

Exemplo incorreto:

```text
helice_volvo.jpg
foto1.jpg
produto.jpg
```

---

## Melhorias Futuras

### Imagens Adicionais

Gerar automaticamente:

```text
Imagem 3 → Aplicações
Imagem 4 → Referências
Imagem 5 → Informações técnicas
```

---

### Marca Automática

Adicionar automaticamente o logotipo do fabricante quando permitido.

---

### Geração por Categoria

Adaptar o layout das imagens conforme a categoria do produto.