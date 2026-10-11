
# Git — Amend, Cherry-pick, Reflog, Blame e Bisect

## 1. git commit --amend

O `--amend` permite modificar o último commit realizado.

É útil para corrigir uma mensagem ou adicionar arquivos esquecidos.

### Alterar a mensagem do último commit

```bash
git commit --amend -m "Nova mensagem"
```

Substitui o último commit por outro com a mensagem corrigida.

### Adicionar um arquivo esquecido

```bash
git add arquivo.txt
git commit --amend --no-edit
```

- `git add` prepara o arquivo.
- `--amend` recria o último commit.
- `--no-edit` mantém a mensagem anterior.

### Atenção

O amend altera o identificador (hash) do commit.

Evite utilizá-lo em commits já compartilhados com outras pessoas.

---

## 2. git cherry-pick

O `git cherry-pick` permite copiar as alterações de um commit específico de outra branch para a branch atual.

É útil quando você deseja aproveitar uma correção sem integrar toda a branch.

### Exemplo prático

Imagine duas branches:

```text
main:    A --- B
                \
login:           C --- D
```

Você quer trazer apenas o commit C para a main.

### Primeiro, entre na branch de destino

```bash
git switch main
```

### Consulte o histórico

```bash
git log --oneline
```

### Aplique o commit desejado

```bash
git cherry-pick abc1234
```

O Git cria um novo commit na branch atual com as alterações selecionadas.

### Em caso de conflito

Depois de resolver os conflitos:

```bash
git add .
git cherry-pick --continue
```

Para cancelar a operação:

```bash
git cherry-pick --abort
```

### Importante

O cherry-pick não move o commit original.

Ele aplica suas alterações em outro lugar, normalmente criando um commit com outro hash.

---

## 3. git reflog

O `git reflog` mostra o histórico local de movimentações das referências do Git, especialmente do HEAD.

É muito útil para recuperar commits após um reset ou uma alteração acidental de branch.

### Consultar o reflog

```bash
git reflog
```

### Exemplo de saída

```text
a1b2c3d HEAD@{0}: reset: moving to HEAD~1
e4f5g6h HEAD@{1}: commit: Cria login
```

Nesse exemplo, o commit do login ainda aparece no reflog.

### Recuperar um commit perdido

```bash
git switch -c recuperacao e4f5g6h
```

Cria uma branch apontando para o commit recuperado.

### Importante

- O reflog é local.
- Não é compartilhado automaticamente com o GitHub.
- Suas entradas podem expirar.
- Nem todo commit perdido poderá ser recuperado indefinidamente.

---

## 4. git blame

O `git blame` mostra qual commit modificou pela última vez cada linha de um arquivo.

É útil para investigar mudanças no código.

### Consultar um arquivo

```bash
git blame app.py
```

### Exemplo de saída

```text
a1b2c3d (Ana 2026-01-10) def login():
e4f5g6h (João 2026-01-12)     validar_usuario()
a1b2c3d (Ana 2026-01-10)     return True
```

O Git apresenta informações como:

- Hash do commit.
- Autor da alteração.
- Data.
- Conteúdo da linha.

### Investigar linhas específicas

```bash
git blame -L 10,20 app.py
```

Mostra a origem das linhas 10 até 20.

### Importante

O blame identifica a última alteração registrada em cada linha.

Ele não determina necessariamente quem criou originalmente o código ou quem causou um problema.

---

## 5. git bisect

O `git bisect` ajuda a descobrir qual commit introduziu um problema.

Ele utiliza uma busca binária no histórico.

Em vez de testar todos os commits, o Git seleciona pontos intermediários para reduzir a quantidade de testes.

### Exemplo prático

Imagine o histórico:

```text
A --- B --- C --- D --- E --- F --- G
```

- No commit A, o sistema funcionava.
- No commit G, o sistema apresenta um erro.

Você deseja descobrir onde o problema começou.

### Iniciar a investigação

```bash
git bisect start
```

### Marcar o commit atual como ruim

```bash
git bisect bad
```

### Informar um commit antigo que funcionava

```bash
git bisect good abc1234
```

O Git selecionará um commit intermediário para teste.

### Se o commit testado funcionar

```bash
git bisect good
```

### Se apresentar o problema

```bash
git bisect bad
```

Repita os testes até o Git identificar o primeiro commit ruim no intervalo investigado.

### Finalizar a investigação

```bash
git bisect reset
```

Retorna ao estado anterior ao início do bisect.

### Automatizar os testes

Também é possível executar testes automaticamente:

```bash
git bisect run python -m pytest
```

O comando de teste deve retornar código 0 quando passar e um código adequado de falha quando o problema estiver presente.

---

## 6. Comparação dos comandos

| Comando | Função |
|---|---|
| `git commit --amend` | Corrige o último commit |
| `git cherry-pick` | Aplica um commit específico |
| `git reflog` | Consulta movimentações locais de referências |
| `git blame` | Investiga a última alteração de cada linha |
| `git bisect` | Localiza o commit que introduziu um problema |

---

## 7. Quando utilizar cada um?

### Amend

Você realizou um commit, mas esqueceu de adicionar um arquivo.

Use:

```bash
git add arquivo.txt
git commit --amend --no-edit
```

### Cherry-pick

Você corrigiu um erro em uma branch e deseja aplicar somente aquela correção em outra.

Use:

```bash
git cherry-pick HASH
```

### Reflog

Você executou um reset e não encontra mais um commit.

Use:

```bash
git reflog
```

### Blame

Você deseja descobrir qual alteração introduziu uma linha específica.

Use:

```bash
git blame arquivo.py
```

### Bisect

Seu projeto funcionava antes, mas agora apresenta um erro e você não sabe qual commit o introduziu.

Use:

```bash
git bisect start
```

---

## 8. Resumo final

- `amend` → Corrige o último commit.
- `cherry-pick` → Aproveita um commit específico.
- `reflog` → Ajuda a recuperar commits e referências.
- `blame` → Investiga a origem das alterações nas linhas.
- `bisect` → Descobre qual commit introduziu um problema.

**Dica:** antes de utilizar comandos que alteram o histórico, consulte `git status` e `git log --oneline`.

**Atenção:** `amend` e `cherry-pick` podem gerar novos hashes. Evite reescrever commits compartilhados sem coordenar com a equipe.
