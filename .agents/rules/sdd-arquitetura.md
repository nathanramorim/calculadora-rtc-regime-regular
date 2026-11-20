---
description: Como cumprir as diretrizes de arquitetura, camadas, recursos estáticos, banco somente leitura e declaração de modelo de módulo.
globs: ["src/main/java/**", "src/main/resources/**", "flyway/**", "sdd/spec/**"]
regras: [ARQ-01, ARQ-02, ARQ-03, ARQ-04, ARQ-05, ARQ-06, ARQ-11, ARQ-12, ARQ-13]
---

# Guia Prático — Arquitetura e Fronteiras de Módulos

Instruções operacionais para os IDs `ARQ-*` de `sdd/memory/constitution.md` (sem reproduzir o texto normativo, `GOV-04`).

## 1. Camadas em `br.gov.serpro.rtc` (`ARQ-01`, `ARQ-02`, `ARQ-06`)
- `api`: controllers, modelos de transporte e interfaces `*ControllerOpenApi`. Sem regra de negócio, sem try-catch de exceção já coberta pelo `@RestControllerAdvice` (ADR007).
- `domain`: entidades, repositórios e serviços. Nunca importa `api`.
- `config`, `core`, `shared`: transversais, sem dependência de `domain`.
- Contrato público novo ou alterado: anotar na interface `*OpenApi` (ADR006); mudança incompatível pede MAJOR e ADR.

## 2. Recursos, dependências e banco (`ARQ-03`, `ARQ-04`, `ARQ-05`)
- XSD e XML em `src/main/resources/xml/<tipo>`; configuração em `application*.yml` por perfil.
- Antes de nova dependência no `pom.xml`, verifique se o JDK ou o Spring já atendem (`ARQ-04`).
- O banco é aberto com `mode=ro` (ADR004). Nenhum `INSERT`, `UPDATE` ou `DELETE` no código da aplicação. Mudança de dados vira script `V####__*.sql` em `flyway/sql/manutencao` (ADR009).

## 3. Cálculo e números (`ARQ-12`, `ARQ-13`)
- Valores fiscais em `BigDecimal`. Arredondamento só por `ArredondamentoUtils` (ADR003). Alterar a classe exige alterar a cópia da `api-simples-nacional`.
- Regra tributária nova ou revisada entra como dado (expressão em `TratamentoTributario`), não como `if` em Java (ADR001).

## 4. Declaração de modelo na Spec (`ARQ-11`)
- Toda spec traz `## Modelo do módulo`: **`enxuto`** para ajuste de dados, texto ou documentação; **`completo`** para regra de negócio nova, mudança de contrato ou de esquema do banco.
