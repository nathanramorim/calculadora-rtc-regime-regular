# Stack — calculadora-rtc-regime-regular

## Dependências

| Camada | Escolha | Versão | Motivo |
|--------|---------|--------|--------|
| Linguagem | Java | 25 | ADR011 |
| Framework | Spring Boot (webmvc, security, validation, data-jpa, cache, actuator) | 4.1.2 | ADR011 |
| Banco | SQLite somente leitura (`sqlite-jdbc`, `hibernate-community-dialects`) | — | ADR004 |
| Migrações | Flyway (plugin Maven, fora do startup) | — | ADR009 |
| Cache | Spring Cache + Caffeine | — | ADR008 |
| Expressões | Apache Commons JEXL3 (sandbox) | — | ADR001 |
| XML | JAXB (`JAXBContext` singleton, `Marshaller` por operação) | — | ADR002 |
| API docs | springdoc-openapi-starter-webmvc-ui | 3.2.0 | ADR006 |
| Observabilidade | Actuator, Micrometer Prometheus, OpenTelemetry | — | ADR010 |
| Mapeamento | ModelMapper, Lombok | — | — |
| Retry | spring-retry | 2.0.12 | — |
| Testes | spring-boot-starter-test, webmvc-test, JaCoCo | — | CI |

## Build e testes
- `./mvnw verify` (Maven Wrapper, Java 25).
- CI: `.github/workflows/publish-jacoco-report.yml` (JDK 25 Temurin, cobertura em PR para `main`).
- Perfil `offline` lê `./calculadora/db/calculadora-pro.db` (`application-offline.yml`).

## Layout do projeto
```
src/main/java/br/gov/serpro/rtc/
  api/        controller, model (transporte), openapi (interfaces *OpenApi), exceptionhandler, util
  domain/     model (entidades), repository, service (cálculo, XML, dados abertos, observabilidade)
  config/     cache, db, retry, security, serializer, xml, download
  core/util   utilitários de núcleo
  shared/util ArredondamentoUtils (idêntica à api-simples-nacional)
src/main/resources/   application-offline.yml, xml/<tipo de documento> (XSD)
src/test/resources/   entradas/ e saidas/ (casos de cálculo), xml/
flyway/sql/           criacao/ (B####) e manutencao/ (V####)
docs/adr/             ADR001 a ADR011
```
