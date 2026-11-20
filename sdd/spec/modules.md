# Módulos — calculadora-rtc-regime-regular

Pacote raiz: `br.gov.serpro.rtc`. Dependências apontam para dentro: `api` → `domain` → `shared`/`core`; `config` é transversal (`ARQ-02`).

## api
**Responsabilidade:** borda HTTP. Valida entrada e traduz para o domínio.
- Controllers: `CalculadoraTributoController`, `BaseCalculoController`, `NfseController`, `PedagioController`, `XMLController`, `DadosAbertosController`, `ObservabilidadeController`, `DownloadController`, `VersaoOfflineController`.
- Anotações OpenAPI em interfaces `*ControllerOpenApi` (ADR006).
- `exceptionhandler`: `@RestControllerAdvice` com `ProblemDetail` (ADR007).

## domain.service — cálculo
**Responsabilidade:** CBS, IBS e Imposto Seletivo.
- `CalculadoraService`, `CalculoTributoService`, `CalculoCbsIbsService`, `CalculoImpostoSeletivoService`, `ProcessamentoItemService`.
- `AvaliadorExpressaoAritmetica` (JEXL3 em sandbox, ADR001); `TratamentoTributarioService` fornece as expressões.
- `ObservabilidadeService`, `ErroService`: resultado por item (ADR010).

## domain.service — XML
**Responsabilidade:** gerar e validar grupos e notas contra XSD.
- `XmlService` e um serviço por documento: `NfeXmlService`, `NfceXmlService`, `CteXmlService`, `CteSimplificadoXmlService`, `Nf3eXmlService`, `BpeXmlService`, `BpeTMXmlService`; `NfseService` para NFS-e.
- `TokenizerService`, `TemplateTokenizerService`, `TokenCacheService` (modelos de texto).

## domain.service — dados abertos
**Responsabilidade:** consulta de tabelas oficiais.
- NCM, NBS, municípios e UF, situações tributárias (CST), classificações (cClassTrib), fundamentações legais, alíquotas (União, UF, município, ad valorem, ad rem), redutores, tipos de DFe.
- `DadosAbertosService`, `VersaoBaseDadosService`, `VersaoAplicacaoService`.

## domain.model e domain.repository
**Responsabilidade:** entidades JPA e repositórios sobre o SQLite somente leitura (ADR004). Consultas cacheadas com `@Cacheable` (ADR008).

## config
**Responsabilidade:** beans transversais.
- `cache` (Caffeine), `db` (SQLite `mode=ro`), `retry`, `security` (cadeia stateless, rotas liberadas), `serializer`, `xml` (`JAXBContext`, ADR002), `download`.

## shared / core
**Responsabilidade:** utilitários sem dependência de domínio.
- `ArredondamentoUtils`: política numérica única (ADR003). Mantida idêntica à da `api-simples-nacional`; alterar aqui exige alterar lá.

## flyway
**Responsabilidade:** criação e manutenção do banco (`B####`, `V####`), executado como etapa explícita (ADR009).
