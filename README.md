```markdown
# Projeto: Agente de Vendas Nível 1 - MDTURBOS

Este repositório contém um frontend (index.html) e integração esperada com um backend (ex.: Google Apps Script) para um agente de vendas N1 que automatiza o primeiro atendimento.

Principais arquivos
- index.html — Interface do agente (suporta execução em Google Apps Script e também como página estática que carrega um CSV).
- ESTOQUE.csv — Planilha de estoque (padrão ponto-e-vírgula).
- scripts/ (opcional) — scripts de limpeza/normalização do CSV (se necessário).

Formato esperado da planilha de estoque (ESTOQUE.csv)
- Separador: ponto-e-vírgula (;)
- Cabeçalho (exatamente estas colunas, nesta ordem recomendada):
  - Nome_Produto
  - PN_Alternativos
  - Modelos
  - Medidas
  - Estoque

Notas sobre conteúdo:
- A coluna `Estoque` deve conter um número (0, 1, 10, ...). O frontend usa esse valor para decisões de disponibilidade.
- As colunas `Modelos` e `Medidas` são livres (texto) e podem conter vírgulas e barras. Não são convertidas automaticamente em tipos numéricos.

Contrato entre frontend (index.html) e backend (Apps Script)
- searchStock(term)
  - Entrada: string (termo de busca)
  - Saída: array de objetos com chaves:
    - Nome_Produto (string)
    - PN_Alternativos (string)
    - Modelos (string)
    - Medidas (string)
    - Estoque (number ou string numérico)
- createLead(payload)
  - Entrada: objeto com chaves: nome, celular, produto_busca, produto_pn, produto_nome, pagamento, status
  - Saída: objeto { ok: true } em caso de sucesso

Sobre hospedagem
- Apps Script: se for usar HtmlService e google.script.run, o index.html funciona integrado ao servidor do Apps Script.
- Hospedagem estática (GitHub Pages): o index.html agora detecta se `google.script.run` existe; caso não exista, carrega ESTOQUE.csv local (ou via endpoint) usando fetch(). Garanta que ESTOQUE_limpo.csv esteja disponível no mesmo diretório público.

Boas práticas
- Mantenha sempre uma cópia original do CSV antes de limpar.
- Valide o campo `Estoque` quando usar dados para vendas (caso haja formatação não padronizada).
- Para deploy público de Web App com escrita (ex.: createLead), proteja endpoints e revise permissões.

Exemplo rápido (local)
- Abrir index.html (ou rodar `python3 -m http.server` e acessar http://localhost:8000).
- Certificar-se que `ESTOQUE_limpo.csv` esteja ao lado do index.html para que a versão estática funcione.

```
