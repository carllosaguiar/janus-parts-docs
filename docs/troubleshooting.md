# Troubleshooting

Este documento reúne os problemas encontrados durante o desenvolvimento e operação do Janus Parts, bem como suas respectivas soluções.

---

## Access Token Inválido

## Sintoma

Ao executar qualquer operação na API do Mercado Livre:

```text
401 Unauthorized
```

ou

```json
{
  "code": "unauthorized",
  "message": "invalid access token"
}
```

---

## Causa

O access token expirou.

---

## Solução

Renovar o token utilizando o refresh token.

Consultar:

```text
autenticacao.md
```

Após atualizar o `.env`, executar novamente:

```bash
python publish.py
```

---

## Produto Não Atualiza

## Sintoma

O anúncio não é atualizado.

Erro:

```text
item.id.invalid
```

ou

```text
Invalid item id
```

---

## Causa

O campo:

```text
ID ML
```

está vazio, incorreto ou contém:

```text
nan
```

---

## Solução

Verificar a coluna:

```text
ID ML
```

na planilha.

Exemplo correto:

```text
MLB6880703648
```

---

## Imagem Não Encontrada

## Sintoma

Erro durante a publicação:

```text
Imagem não encontrada
```

---

## Causa

Não existe arquivo correspondente ao código do produto.

---

## Exemplo

Produto:

```text
113320
```

Imagem esperada:

```text
imagens/113320.jpg
```

ou

```text
imagens/113320.png
```

---

## Solução

Adicionar a imagem correta na pasta:

```text
imagens/
```

---

## Coluna ID ML Não Encontrada

## Sintoma

Erro:

```text
KeyError: 'ID ML'
```

---

## Causa

A planilha não possui a coluna:

```text
ID ML
```

---

## Solução

Adicionar a coluna exatamente com este nome:

```text
ID ML
```

Exemplo:

| CODIGO MERC. | DESCRICAO MERCADORIA | ID ML |
|-------------|----------------------|--------|
| 113320 | HELICE MOTOR VISCOSA | MLB6880703648 |

---

## Produto Criado Novamente

## Sintoma

O sistema cria um novo anúncio em vez de atualizar um existente.

---

## Causa

O campo:

```text
ID ML
```

está vazio.

---

## Solução

Verificar se o anúncio já existe.

Caso exista, preencher corretamente o ID.

Exemplo:

```text
MLB6902472512
```

---

## Mercado Envios Não Aparece

## Sintoma

O anúncio exibe:

```text
Entrega a combinar com o vendedor
```

---

## Causa

O anúncio não está utilizando:

```text
ME2
```

ou a conta ainda não possui Mercado Envios habilitado.

---

## Verificação

Executar:

```python
service.obter_shipping()
```

ou consultar diretamente a API.

Resultado esperado:

```json
{
  "modes": [
    "custom",
    "not_specified",
    "me2"
  ]
}
```

---

## Solução

Habilitar Mercado Envios na conta Mercado Livre.

Após isso:

```bash
python publish.py
```

para atualizar os anúncios.

---

## Mercado Envios Não Atualiza

## Sintoma

Mesmo após habilitar o Mercado Envios, o anúncio continua exibindo:

```text
Entrega a combinar com o vendedor
```

---

## Solução

Executar novamente:

```bash
python publish.py
```

O método:

```python
atualizar_shipping()
```

irá atualizar os anúncios existentes.

---

## Imagem Rejeitada Pelo Mercado Livre

## Sintoma

Recebimento de e-mail do Mercado Livre informando problemas na imagem.

Exemplos:

```text
Imagem com logotipo
Imagem com texto
Imagem com marca d'água
```

---

## Causa

A imagem principal não segue as políticas da plataforma.

---

## Solução

Utilizar:

```text
Imagem 1 → Produto limpo
Imagem 2 → Institucional
```

A imagem principal deve conter apenas o produto.

---

## Produto Pequeno na Imagem

## Sintoma

O produto aparece muito pequeno na imagem final.

---

## Causa

O algoritmo de recorte removeu áreas importantes da imagem.

---

## Solução

Remover o processamento excessivo de recorte.

Utilizar apenas:

```python
ImageOps.contain()
```

ou

```python
ImageOps.pad()
```

para centralização.

---

## Erro Durante Upload da Imagem

## Sintoma

Falha durante:

```text
upload_imagem()
```

---

## Possíveis Causas

- token expirado;
- arquivo inexistente;
- imagem corrompida;
- indisponibilidade temporária da API.

---

## Solução

Verificar:

```text
1. Token
2. Caminho do arquivo
3. Integridade da imagem
4. Status da API
```

---

## Planilha Atualizada Mas Anúncio Não Mudou

## Sintoma

Preço ou estoque alterados na planilha não aparecem no anúncio.

---

## Causa

O publish não foi executado.

---

## Solução

Executar:

```bash
python publish.py
```

---

## Estoque Não Atualiza

## Sintoma

Erro durante:

```python
atualizar_estoque()
```

---

## Possíveis Causas

- ID ML incorreto;
- token inválido;
- anúncio inexistente.

---

## Solução

Verificar:

```text
ID ML
Access Token
Existência do anúncio
```

---

## Publish Interrompido

## Sintoma

Execução interrompida antes do final.

---

## Possíveis Causas

- exceção não tratada;
- problema de rede;
- erro em serviço externo.

---

## Solução

Verificar o traceback exibido no terminal.

O publish atual utiliza:

```python
try:
    ...
except Exception:
    ...
```

permitindo que os demais produtos continuem sendo processados.

---

## Resultado Esperado

Ao final da execução:

```text
============================================================
SUCESSOS: 56
CRIADOS: 0
ATUALIZADOS: 56
ERROS: 0
PROCESSAMENTO FINALIZADO
```

---

## Histórico de Problemas Resolvidos

Durante a implantação inicial do Janus Parts foram resolvidos:

- token expirado;
- ausência da coluna ID ML;
- anúncios sendo recriados;
- Mercado Envios desabilitado;
- imagens rejeitadas pelo Mercado Livre;
- problemas de alinhamento das imagens;
- sincronização automática de estoque;
- sincronização automática de preço;
- gravação automática do ID ML.

Todos esses cenários já possuem solução documentada neste arquivo.