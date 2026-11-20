---
description: Como aplicar validação de entrada, higiene de segredos e logs e governança de dependências no projeto Java.
globs: ["**/*"]
regras: [SEG-01, SEG-02, SEG-04, SEG-05, SEG-07]
---

# Guia Prático — Segurança, Segredos e Dependências

Procedimentos para `SEG-01`, `SEG-02`, `SEG-04`, `SEG-05` e `SEG-07` (`GOV-04`).

## 1. Entrada e escrita (`SEG-01`, `SEG-02`)
- Valide toda entrada na borda com Bean Validation nos modelos de `api/model`. Falha de validação responde 422.
- Expressões JEXL rodam só em `JexlSandbox`, com variáveis `BigDecimal`. Nunca avalie texto vindo da requisição.
- XML recebido é validado contra o XSD antes do uso. Desabilite entidades externas (XXE) em qualquer parser novo.
- Caminhos de arquivo (download, banco) vêm de configuração, nunca da requisição; rejeite `..` e separadores inesperados.
- A aplicação não escreve no banco de referência (`mode=ro`).

## 2. Segredos e logs (`SEG-04`, `SEG-05`)
- Sem credenciais, tokens ou chaves em código, `application*.yml` ou testes. Use variáveis de ambiente e placeholders fictícios.
- Logs e erros não despejam payload fiscal completo, cabeçalhos de autenticação nem variáveis de ambiente.
- A `WebSecurityConfig` atual libera todas as rotas e vale só para homologação. Qualquer exposição pública exige revisão e ADR.

## 3. Dependências (`SEG-07`)
- Antes de alterar o `pom.xml`: confirme que JDK ou Spring não atendem; consulte o context7 (se `ativo` em `sdd/memory/mcps.md`) na versão exata.
- Preencha `## Dependências novas` na Spec com artefato, versão, licença e motivo, ou registre `Nenhuma.`.
