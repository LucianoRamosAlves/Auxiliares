
# Git — Ignore, Stash, Reset e Revert

## 1. .gitignore

O `.gitignore` é um arquivo que informa ao Git quais arquivos e pastas devem ser ignorados.

É útil para evitar o versionamento de:
- Ambientes virtuais.
- Senhas e arquivos `.env`.
- Arquivos temporários.
- Pastas de cache.
- Arquivos gerados automaticamente.

### Exemplo de .gitignore para Python

```gitignore
# Ambiente virtual
.venv/
venv/

# Variáveis de ambiente
.env

# Cache do Python
__pycache__/
*.pyc

# Arquivos temporários
*.log

# Configurações do editor
.vscode/
```

### Importante

O `.gitignore` não ignora arquivos que já estão sendo rastreados pelo Git.

Para parar de rastrear um arquivo sem apagá-lo do computador:

```bash
git rm --cached arquivo.txt
```

Para uma pasta:

```bash
git rm -r --cached pasta/
```

Depois, registre a alteração com um commit.

---

## 2. git stash

O `git stash` guarda temporariamente alterações que ainda não foram registradas em um commit.

É útil quando você precisa trocar de branch, mas ainda não terminou seu trabalho.

### Guardar alterações

```bash
git stash
```

Guarda alterações rastreadas no stash e limpa essas alterações da área de trabalho.

### Guardar com uma descrição

```bash
git stash push -m "Tela de login"
```

### Listar alterações guardadas

```bash
git stash list
```

### Recuperar alterações

```bash
git stash pop
```

Aplica o stash mais recente e, se a aplicação for bem-sucedida, remove sua entrada da lista.

### Recuperar sem remover

```bash
git stash apply
```

Aplica o stash, mas mantém sua entrada guardada.

### Excluir um stash

```bash
git stash drop
```

### Guardar também arquivos novos

```bash
git stash -u
```

Inclui arquivos não rastreados, mas não os ignorados.

### Exemplo prático

Você está desenvolvendo uma tela de login e precisa corrigir um problema na branch principal.

```bash
git stash
git switch main

# Corrigir o problema

git switch login
git stash pop
```

Assim, você pode continuar de onde parou.

---

## 3. git reset

O `git reset` permite redefinir a posição da branch atual ou retirar alterações da área de preparação.

Dependendo da opção, pode preservar ou descartar alterações.

### Retirar arquivo da área de preparação

```bash
git reset arquivo.txt
```

Retira o arquivo do staging sem apagar suas alterações.

Alternativa moderna:

```bash
git restore --staged arquivo.txt
```

### git reset --soft

```bash
git reset --soft HEAD~1
```

Desfaz o último commit, mas mantém suas alterações na área de preparação.

### git reset --mixed

```bash
git reset --mixed HEAD~1
```

Desfaz o último commit e retira suas alterações da área de preparação.

Os arquivos modificados continuam no computador.

É o modo padrão do `git reset`.

### git reset --hard

```bash
git reset --hard HEAD~1
```

Desfaz o último commit e descarta alterações rastreadas na área de trabalho e no staging.

**Cuidado:** pode causar perda de trabalho.

### O que significa HEAD~1?

- `HEAD` → commit atual.
- `HEAD~1` → primeiro ancestral do commit atual.
- `HEAD~2` → segundo ancestral do commit atual.

---

## 4. git revert

O `git revert` desfaz as alterações introduzidas por um commit criando um novo commit.

Diferentemente do `reset`, ele preserva o histórico existente.

### Reverter o último commit

```bash
git revert HEAD
```

### Reverter um commit específico

```bash
git revert abc1234
```

O Git cria um novo commit que tenta desfazer as alterações daquele commit.

Podem ocorrer conflitos, que precisam ser resolvidos.

### Exemplo prático

Imagine o histórico:

```text
A — B — C
```

O commit C apresentou um problema.

Executando:

```bash
git revert HEAD
```

O histórico passa a ser:

```text
A — B — C — D
```

O commit D desfaz as alterações de C, preservando o histórico.

---

## 5. Reset vs Revert

| Característica | Reset | Revert |
|---|---|---|
| Desfaz commits | Sim | Sim |
| Cria novo commit | Não | Sim |
| Preserva o histórico existente | Pode reescrever | Sim |
| Pode descartar alterações locais | Sim | Não por padrão |
| Indicado para commits compartilhados | Geralmente não | Sim |

### Quando usar?

**Reset:**
- Corrigir commits locais.
- Retirar arquivos do staging.
- Reorganizar alterações ainda não compartilhadas.

**Revert:**
- Desfazer alterações já publicadas.
- Corrigir problemas mantendo o histórico.
- Trabalhar com segurança em equipe.

---

## 6. Resumo dos comandos

| Comando | Função |
|---|---|
| `.gitignore` | Define arquivos ignorados |
| `git stash` | Guarda alterações temporariamente |
| `git stash list` | Lista alterações guardadas |
| `git stash pop` | Recupera e remove o stash aplicado |
| `git stash apply` | Recupera sem remover o stash |
| `git stash drop` | Exclui uma entrada do stash |
| `git reset --soft` | Move HEAD e mantém staging |
| `git reset --mixed` | Move HEAD e limpa staging |
| `git reset --hard` | Move HEAD e descarta alterações rastreadas |
| `git revert HEAD` | Reverte o último commit criando outro |

## 7. Dicas importantes

1. Use `.gitignore` desde o início do projeto.
2. Use `git stash` para guardar trabalhos inacabados.
3. Prefira `git restore --staged` para retirar arquivos do staging.
4. Evite `git reset --hard` sem verificar suas alterações.
5. Prefira `git revert` para desfazer commits já compartilhados.
6. Consulte `git status` antes de executar comandos destrutivos.

## Resumo final

- `.gitignore` → Ignora arquivos.
- `git stash` → Guarda alterações temporariamente.
- `git reset` → Redefine commits ou staging.
- `git revert` → Desfaz alterações criando um novo commit.

**Regra prática:** se o commit já foi compartilhado com outras pessoas, normalmente prefira `git revert` em vez de reescrever o histórico com `git reset`.
