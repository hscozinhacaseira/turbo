# Projeto: Agente de Vendas Nível 1 - MDTURBOS

Este repositório contém a interface (index.html) e um exemplo de backend em Google Apps Script (Code.gs) para um agente de vendas N1 que automatiza o primeiro atendimento.

## Estrutura necessária nas Planilhas (Google Sheets)

1. Planilha "Estoque" (obrigatória)
   - Primeira linha: cabeçalhos
   - Colunas esperadas (nomes exatos recomendados):
     - ID_Produto
     - Nome_Produto
     - PN_Alternativos
     - Modelos
     - Medidas
     - Estoque
   - O campo `Estoque` deve conter número (0, 1, 10, ...).

2. Planilha "Leads" (será criada automaticamente pelo script se não existir)
   - Cabeçalho sugerido: created_at, nome, celular, produto_busca, produto_pn, produto_nome, pagamento, status

## Contrato entre frontend (index.html) e backend (Apps Script)
- searchStock(term)
  - Entrada: string (termo de busca)
  - Saída: array de objetos com chaves:
    - Nome_Produto (string)
    - PN_Alternativos (string)
    - Modelos (string)
    - Medidas (string)
    - Estoque (number)

- createLead(payload)
  - Entrada: objeto com chaves: nome, celular, produto_busca, produto_pn, produto_nome, pagamento, status
  - Saída: objeto { ok: true } em caso de sucesso

## Deploy (Google Apps Script)
1. Crie um novo projeto em Google Apps Script e cole `Code.gs` e o HTML (`index.html`) com o mesmo nome usado em doGet.
2. Em "Publicar" -> "Deploy as web app" (ou botão Deploy > New deployment):
   - Execute como: "Me"
   - Acesso: escolher de acordo com necessidade (ex.: "Anyone" para acesso público)
3. Se optar por hospedar fora do Apps Script, substitua chamadas `google.script.run` por `fetch()` para o Web App URL e ajuste headers/CORS conforme necessário.

## Observações e boas práticas
- Valide dados antes de gravar no sheet (ex.: formato de telefone).
- Log de erros no Apps Script ajuda a diagnosticar problemas (Use `console.error` que aparece em Executions).
- Se for expor o Web App publicamente, avalie autenticação e limites de escrita na planilha.

## Exemplo de fluxo
1. Front pergunta nome -> grava localmente.
2. Pergunta celular -> grava.
3. Pergunta termo do produto -> chama `searchStock(term)`.
4. Se múltiplos resultados -> lista e permite seleção.
5. Ao confirmar orçamento -> chama `createLead` com os dados finais.

