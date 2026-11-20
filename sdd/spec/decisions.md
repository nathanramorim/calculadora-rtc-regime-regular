# Decisões — calculadora-rtc-regime-regular

Os ADRs do projeto vivem em `docs/adr/` (fonte única). As decisões do método SDD vivem em `sdd/spec/adr/`.

## Resolvidas

| # | Decisão | Resolução | Motivo | ADR |
|---|---------|-----------|--------|-----|
| D1 | Cálculo tributário | Expressões JEXL3 em `TratamentoTributario`, sandbox | Regras mudam por lei; auditáveis nos dados | [ADR001](../../docs/adr/ADR001-calculo-tributario-por-expressao.md) |
| D2 | XML | `JAXBContext` singleton, `Marshaller` por operação | Thread-safety | [ADR002](../../docs/adr/ADR002-padrao-jaxb-thread-safe.md) |
| D3 | Precisão numérica | `ArredondamentoUtils`, `HALF_EVEN`, resposta fecha consigo mesma | Evitar rejeição 1091 | [ADR003](../../docs/adr/ADR003-estrategia-de-precisao-numerica.md) |
| D4 | Banco | SQLite `mode=ro` | Dados de referência imutáveis em runtime | [ADR004](../../docs/adr/ADR004-banco-sqlite-somente-leitura.md) |
| D5 | Plataforma | Spring Boot 4.1.2 e Java 25 (substitui ADR005) | Versão vigente | [ADR011](../../docs/adr/ADR011-adocao-spring-boot-4-java-25.md) |
| D6 | Documentação da API | SpringDoc, code-first, anotações em interfaces `*OpenApi` | Controllers limpos | [ADR006](../../docs/adr/ADR006-documentacao-api-com-springdoc.md) |
| D7 | Erros | `@RestControllerAdvice` + `ProblemDetail` | Contrato único | [ADR007](../../docs/adr/ADR007-estrategia-de-tratamento-de-erros.md) |
| D8 | Cache | Spring Cache + Caffeine | Dados de referência | [ADR008](../../docs/adr/ADR008-estrategia-de-cache-com-caffeine.md) |
| D9 | Migrações | Flyway Maven, fora do startup | Sem migração automática | [ADR009](../../docs/adr/ADR009-migracoes-e-versionamento-do-banco.md) |
| D10 | Observabilidade | Falha por item, threads virtuais, tracing OTel | Um item não derruba a operação | [ADR010](../../docs/adr/ADR010-estrategia-de-observabilidade.md) |
| D11 | Método SDD | Forge-SDD, com `docs/adr/` como fonte dos ADRs do projeto | Rastreabilidade de spec e portões | [ADR-0001](adr/adr-0001-adocao-forge-sdd.md) |

ADR005 (Spring Boot 3 e Java 21) está substituída pela ADR011.

## Abertas

| # | Questão |
|---|---------|
| D12 | O ADR003 está datado de 17/11/2026, posterior aos demais (26/05/2026). Confirmar a data com o mantenedor. |
| D13 | ADR005 e ADR011 não aparecem na mesma lista de `docs/adr/ADRs.md` (ADR005 ausente dos aceitos). Decidir se ADR005 entra numa seção "Substituídos". |
| D14 | `WebSecurityConfig` libera todas as rotas (versão de homologação). Definir a política para uso público. |
