# 25 - Validacao autonoma de projetos legados

## 1. Objetivo

Definir como um agente externo testa um projeto legado sem contaminar o
blueprint, o repositorio-fonte do Harness ou a memoria do alvo com suposicoes
do operador.

Este procedimento vale depois da decisao `ready_for_target_use` da versao
liberada do Harness. Antes disso, apenas fixtures e a construcao/verificacao
do proprio Harness podem ser usadas.

## 2. Identidade obrigatoria

A nova sessao deve abrir o repositorio-alvo como workspace primario.

Ela deve ler, nesta ordem:

1. `.harness/AGENTS.md`
2. `.harness/project-manifest.yaml`
3. `.harness/TARGET_CONTEXT.yaml`, quando existir
4. `.harness/MEMORY.md`
5. `.harness/LOG.md`
6. `.harness/blueprint/` como referencia local da versao usada
7. `docs/` e artefatos canonicos do projeto-alvo

O agente deve confirmar Git root, remote, branch, estado limpo ou mudancas
preexistentes, `workspace_kind=managed_target`, work mode e versao pinned do
Harness.

Uma sessao nao pode selecionar `harness_build` em um projeto-alvo nem concluir
uma analise com base apenas no nome da pasta ou no historico da conversa.

## 3. Fronteiras de isolamento

A copia em `.harness/blueprint/` pertence ao projeto. Seus resultados podem
mudar a memoria, baseline, propostas e evidencias locais.

Ela nao deve ser usada para alterar diretamente:

- o repositorio central `portable-engineering-harness`
- o blueprint oficial
- outro projeto-alvo
- produtos ou subsistemas nao solicitados

Observacoes sao aprovadas apenas localmente. Promover uma evidencia ou padrao ao
blueprint oficial exige generalizacao, revisao, rastreabilidade e aprovacao.

## 4. Reconcilacao automatica

Antes de testar, o agente verifica Java/JDK, Gradle wrapper, Android SDK,
`ANDROID_HOME` ou `sdk.dir`, ADB, emulador, Maestro, Firebase/keystore apenas
como presenca/configuracao e ferramentas instaladas.

Se a configuracao estiver ausente ou contraditoria:

- registrar `blocked` ou `needs_input`
- registrar a verificacao feita
- nao representar ausencia de infraestrutura como falha do aplicativo
- nao modificar codigo para contornar limitacao de ambiente

## 5. Runner deterministico

O alvo pode conter um runner ignorado pelo Git, por exemplo em
`.harness-local/run-validation.ps1`.

O runner deve:

1. criar um `run_id`
2. registrar workspace, work mode, commit e objetivo
3. validar ferramentas
4. registrar versoes
5. executar os testes aprovados
6. capturar exit codes e artefatos
7. persistir checkpoint/handoff
8. calcular hashes dos resultados
9. terminar em estado explicito

O runner coleta fatos. O agente interpreta resultados, decide o proximo teste
e separa fatos observados de hipoteses.

## 6. Sequencia inicial recomendada

Para projetos Android/Kotlin:

```powershell
.\gradlew.bat testDebugUnitTest --console=plain
.\gradlew.bat assembleDebug --console=plain
```

Depois, quando emulator e ADB estiverem confirmados:

```powershell
adb devices
maestro test .maestro/
```

Cada comando registra comando exato, diretorio, hora, exit code, versoes,
arquivos/logs gerados e SHA-256 dos artefatos.

Se somente o SDK estiver faltando, a etapa correta e `blocked`; nao executar
Maestro contra um APK que nao foi construído.

## 7. Evidencias persistentes

Cada run no alvo deve persistir no minimo:

- `.harness/runs/<run-id>/run.yaml`
- `.harness/runs/<run-id>/events.jsonl`
- `.harness/runs/<run-id>/checkpoint.json`
- `.harness/runs/<run-id>/evidence-index.json`
- `.harness/runs/<run-id>/handoff.md`

Atualizacoes posteriores mantem:

- `.harness/MEMORY.md`
- `.harness/LOG.md`
- baseline/local status
- artifacts canonicos do projeto

Uma conversa ou resumo do agente nao substitui essas evidencias.

## 8. Classificacao de resultados

Usar classes explicitas:

- `passed`
- `failed`
- `observed`
- `blocked`
- `needs_input`
- `not_applicable`
- `unknown`

Proibicoes:

- nao chamar sucesso o exit code de infraestrutura
- nao confundir teste nao executado com teste aprovado
- nao inferir comportamento do produto por codigo apenas
- nao omitir logs de falha
- nao esconder limites do emulador ou SDK

## 9. Revisao independente

Quando houver implementacao ou mudanca no alvo:

- um agente especialista faz a alteracao
- um agente separado e sem write scope revisa
- QA/evidencia nao pertence ao implementador
- findings bloqueantes impedem encerramento
- arquitetura reservada exige o ator autorizado

O Master coordena o processo, mas nao substitui conclusoes, testes ou revisao
dos especialistas.

## 10. Estados finais

Um run termina em `completed`, `failed`, `blocked`, `needs_input` ou
`waiting_for_request`.

O alvo nao recebe feature por inferencia. Toda entrega exige pedido explicito,
baseline aprovado ou analise as-is, arquitetura/compatibilidade verificadas,
testes, review independente e boundary pre-producao preservado.

## 11. Criterio de operacionalizacao

Este procedimento esta apto para uso autonomo quando existem runner
persistente, executor supervisionavel, schemas/checks, write scope isolado,
memoria local duravel, test-first, review independente e comunicacao de blocker
sem adivinhar.

Uma pessoa pode abrir nova sessao manualmente, mas autonomia reprodutivel
depende do runner e dos artefatos duraveis, nao da memoria de conversa.
