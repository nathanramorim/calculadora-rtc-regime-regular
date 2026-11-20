# Catálogo de Conceitos e Contratos (`DUP-01`) — calculadora-rtc-regime-regular

Regra `DUP-01`: **Um conceito = uma casa.** Consulte esta tabela antes de criar entidade, módulo ou utilitário (`DUP-02`).

| Conceito | Pacote/Domínio Dono | Entidade/Classe Mestra | Contrato Público | Observação |
| --- | --- | --- | --- | --- |
| Arredondamento e escala fiscal | `shared.util` | `ArredondamentoUtils` | `MODO`, `ESCALA_VALOR_FISCAL` e métodos de rateio | Idêntica à da `api-simples-nacional` (ADR003) |
| Fórmulas tributárias | `domain.model` / `domain.service` | `TratamentoTributario`, `AvaliadorExpressaoAritmetica` | colunas `TRTR_EXPRESSAO_*` | Regra nos dados (ADR001) |
| Cálculo CBS/IBS | `domain.service` | `CalculoCbsIbsService` | via `CalculoTributoService` | Orquestrado por `CalculadoraService` |
| Imposto Seletivo | `domain.service` | `CalculoImpostoSeletivoService` | via `CalculoTributoService` | — |
| Processamento por item | `domain.service` | `ProcessamentoItemService` | estado `CALCULADO` ou erro | ADR010 |
| Erros de domínio e códigos | `domain.service` / `api.exceptionhandler` | `ErroService`, handler global | `ProblemDetail`, código `REG-NNN` | ADR007 |
| Geração e validação de XML | `domain.service` | `XmlService` e um serviço por tipo | `/xml/generate`, `/xml/validate` | ADR002 |
| Classificação tributária (CST, cClassTrib) | `domain.service` | `ClassificacaoTributariaService`, `SituacaoTributariaService` | `/dados-abertos/...` | — |
| Alíquotas de referência | `domain.service` | `AliquotaReferenciaService` e correlatos | `/dados-abertos/aliquota-*` | — |
| Configuração de cache | `config.cache` | beans Caffeine | `application*.yml` | ADR008 |
| Esquema do banco | `flyway/sql` | scripts `B####` e `V####` | Flyway Maven | ADR009 |
