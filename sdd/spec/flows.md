# Fluxos — calculadora-rtc-regime-regular

## 1. Cálculo no regime geral (`POST /calculadora/regime-geral`)
```
Requisição JSON
  ↓ validação na borda (Bean Validation) → 422 em erro
Validação global (campos, UF, município)
  ↓
Para cada item (sequencial neste endpoint; falha interrompe a operação)
  ↓ classificação tributária → TratamentoTributario → expressões JEXL
  ↓ ArredondamentoUtils (HALF_EVEN, fecha com somas)
Totais da operação
  ↓
Resposta JSON
```

## 2. Cálculo com observabilidade (`POST /observabilidade/regime-geral`)
```
Validação global (falha → 422)
  ↓
Itens em paralelo (threads virtuais); cada item isolado
  ↓ estado CALCULADO ou estado de erro com código (ex.: REG-011)
Resposta com estado e erros por item; demais itens seguem (ADR010)
```
Código de erro explicado em `GET /observabilidade/erros?codigo=...`.

## 3. Base de cálculo unificada
`POST /base-calculo/base-calculo-unificada` (NF-e/NFC-e) e `.../base-calculo-unificada-nfse` (NFS-e): componentes da base consolidados e verificados contra inconsistência.

## 4. Geração e validação de XML
```
Resultado do cálculo
  ↓ POST /xml/generate?tipo=<nfe|nfce|cte|...>
Serviço XML do tipo → JAXB (Marshaller por operação) → XML do grupo
  ↓ POST /xml/validate?tipo=...&subtipo=grupo|nota
Validação contra o XSD embutido → resultado
```

## 5. Dados abertos
`GET /dados-abertos/...` → serviço → repositório (SQLite `mode=ro`, cache Caffeine) → JSON.

## 6. Atualização da base de dados
```
Nova tabela oficial / mudança legislativa
  ↓ script V####__*.sql em flyway/sql/manutencao
  ↓ Flyway (fora do startup) gera novo arquivo SQLite versionado
  ↓ arquivo substitui o do ambiente
```

## 7. Tratamento de erro
Qualquer exceção sobe ao `@RestControllerAdvice`: validação 422, não encontrado 404, JSON inválido 400, interno 500, sempre `ProblemDetail`.
