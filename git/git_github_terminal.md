
# Git e GitHub — Fluxo Completo pelo Terminal

## 1. Git x GitHub

**Git:** sistema de controle de versões que funciona localmente.

**GitHub:** plataforma que hospeda repositórios Git na internet.

O GitHub Desktop é uma interface gráfica que facilita operações que também podem ser realizadas pelo terminal.

---

## 2. git init — Iniciar repositório

Transforma a pasta atual em um repositório Git.

```bash
git init
```

Para verificar o estado:

```bash
git status
```

---

## 3. git remote — Conectar ao GitHub

Um remote é uma referência a outro repositório, normalmente hospedado no GitHub.

### Adicionar um remote

```bash
git remote add origin https://github.com/usuario/projeto.git
```

- `remote` → gerencia repositórios remotos.
- `add` → adiciona uma conexão.
- `origin` → nome convencional do remote.
- URL → endereço do repositório.

### Visualizar os remotes

```bash
git remote -v
```

Exemplo:

```text
origin  https://github.com/usuario/projeto.git (fetch)
origin  https://github.com/usuario/projeto.git (push)
```

### Alterar a URL

```bash
git remote set-url origin NOVA_URL
```

### Remover um remote

```bash
git remote remove origin
```

**Importante:** `origin` é apenas um nome convencional. Não é obrigatório.

---

## 4. git clone — Clonar um repositório

Baixa um repositório existente do GitHub para o computador.

```bash
git clone https://github.com/usuario/projeto.git
```

O clone normalmente configura automaticamente o remote `origin`.

Para entrar na pasta:

```bash
cd projeto
```

---

## 5. git status — Verificar alterações

```bash
git status
```

Mostra:

- Arquivos modificados.
- Arquivos novos.
- Arquivos excluídos.
- Alterações preparadas para commit.
- Informações sobre a branch atual.

É equivalente à visualização de alterações do GitHub Desktop.

---

## 6. git diff — Comparar alterações

Mostra diferenças entre versões dos arquivos.

### Alterações ainda não preparadas

```bash
git diff
```

### Alterações preparadas com git add

```bash
git diff --staged
```

### Comparar dois commits

```bash
git diff HASH1 HASH2
```

### Comparar duas branches

```bash
git diff main login
```

### Mostrar somente arquivos alterados

```bash
git diff --name-only
```

**Importante:** `git diff` sem argumentos não mostra automaticamente todas as alterações, como arquivos novos não rastreados.

---

## 7. git add — Preparar alterações

Adiciona alterações à área de preparação (staging).

### Adicionar tudo

```bash
git add .
```

### Adicionar um arquivo

```bash
git add app.py
```

### Retirar do staging

```bash
git restore --staged app.py
```

---

## 8. git commit — Salvar uma versão

Registra as alterações preparadas no histórico local.

```bash
git commit -m "Cria tela de login"
```

Consultar os commits:

```bash
git log --oneline
```

**Importante:** o commit ainda não envia nada ao GitHub.

---

## 9. git push — Enviar ao GitHub

Envia commits locais para um repositório remoto.

### Primeiro envio de uma branch

```bash
git push -u origin main
```

- `push` → envia commits.
- `-u` → configura a branch de acompanhamento (upstream).
- `origin` → repositório remoto.
- `main` → branch enviada.

Depois de configurar o upstream, normalmente basta:

```bash
git push
```

### Enviar outra branch

```bash
git push -u origin login
```

**Atenção:** o push pode ser recusado se o remoto tiver alterações que precisam ser integradas primeiro.

---

## 10. git fetch — Buscar atualizações

Busca commits e referências do repositório remoto.

```bash
git fetch origin
```

O fetch atualiza informações como `origin/main`, mas não integra automaticamente as alterações à branch local.

### Buscar todos os remotes

```bash
git fetch --all
```

### Comparar a branch local com a remota

```bash
git log --oneline HEAD..origin/main
```

Mostra commits presentes em `origin/main` e ausentes da branch atual.

**Dica:** use `fetch` quando quiser verificar atualizações antes de integrá-las.

---

## 11. git pull — Baixar e integrar

Busca alterações remotas e tenta integrá-las à branch atual.

```bash
git pull
```

O pull normalmente executa:

1. Fetch — busca as atualizações.
2. Merge ou rebase — integra as alterações, conforme a configuração.

### Exemplo explícito com merge

```bash
git pull --no-rebase origin main
```

### Exemplo com rebase

```bash
git pull --rebase origin main
```

**Importante:** se houver conflitos ou alterações locais incompatíveis, pode ser necessário resolver os problemas antes de continuar.

---

## 12. Diferença entre Fetch, Pull e Push

| Comando | Função |
|---|---|
| `git fetch` | Busca atualizações sem integrar |
| `git pull` | Busca e integra atualizações |
| `git push` | Envia commits locais |

### Exemplo visual

```text
         GITHUB
            |
            | git fetch
            v
   Referências remotas locais
            |
            | merge / rebase
            v
       BRANCH LOCAL
            |
            | git push
            v
         GITHUB
```

---

## 13. git branch — Gerenciar branches

### Listar branches locais

```bash
git branch
```

### Listar branches locais e remotas

```bash
git branch -a
```

### Criar e entrar em uma branch

```bash
git switch -c login
```

### Trocar de branch

```bash
git switch main
```

### Integrar uma branch

```bash
git merge login
```

### Excluir uma branch local integrada

```bash
git branch -d login
```

### Excluir uma branch remota

```bash
git push origin --delete login
```

---

## 14. git restore — Descartar alterações

Restaura arquivos rastreados.

### Descartar alterações não preparadas

```bash
git restore app.py
```

### Retirar arquivo do staging

```bash
git restore --staged app.py
```

**Cuidado:** restaurar um arquivo pode descartar alterações locais não salvas em commits.

---

## 15. git log — Consultar histórico

### Histórico completo

```bash
git log
```

### Histórico resumido

```bash
git log --oneline
```

### Histórico com branches

```bash
git log --oneline --graph --all --decorate
```

Esse último comando permite visualizar melhor a estrutura de commits e branches.

---

## 16. GitHub Desktop x Terminal

| GitHub Desktop | Terminal |
|---|---|
| Clone repository | `git clone` |
| Changes | `git status` e `git diff` |
| Stage changes | `git add` |
| Commit | `git commit` |
| Push origin | `git push` |
| Fetch origin | `git fetch origin` |
| Pull origin | `git pull` |
| Current branch | `git branch` |
| New branch | `git switch -c` |
| Switch branch | `git switch` |
| Merge into current branch | `git merge` |
| Discard changes | `git restore` |
| History | `git log` |

---

## 17. Fluxo completo — Projeto novo

Imagine que você criou um sistema de estacionamento em Flask e deseja publicá-lo no GitHub.

### Passo 1 — Entrar na pasta

```bash
cd estacionamento
```

### Passo 2 — Inicializar Git

```bash
git init
```

### Passo 3 — Configurar a branch principal

```bash
git branch -M main
```

### Passo 4 — Preparar arquivos

Antes, configure o `.gitignore` para evitar enviar arquivos sensíveis.

```bash
git add .
```

### Passo 5 — Criar o primeiro commit

```bash
git commit -m "Primeira versão do estacionamento"
```

### Passo 6 — Conectar ao GitHub

Crie um repositório vazio no GitHub.

Depois:

```bash
git remote add origin https://github.com/usuario/estacionamento.git
```

### Passo 7 — Enviar

```bash
git push -u origin main
```

Pronto! O projeto estará publicado no GitHub.

**Observação:** o GitHub pode solicitar autenticação. Para HTTPS, normalmente utiliza-se um gerenciador de credenciais ou token, não a senha da conta.

---

## 18. Fluxo diário — Projeto existente

### Antes de começar

```bash
git status
git fetch origin
git pull
```

### Depois de programar

```bash
git status
git diff
git add .
git commit -m "Implementa saída de veículos"
git push
```

Esse é o fluxo básico que você normalmente realiza pelo GitHub Desktop.

---

## 19. Resolvendo conflitos

Um conflito pode acontecer quando duas alterações incompatíveis afetam o mesmo trecho de código.

Exemplo:

```text
<<<<<<< HEAD
print("Olá")
=======
print("Bem-vindo")
>>>>>>> outra-branch
```

Você precisa editar o arquivo e escolher o conteúdo correto, removendo os marcadores.

Depois de resolver um conflito de merge:

```bash
git add arquivo.py
git commit
```

Em um rebase, normalmente utiliza-se:

```bash
git add arquivo.py
git rebase --continue
```

---

## 20. Comandos extras úteis

| Comando | Função |
|---|---|
| `git remote -v` | Mostra conexões remotas |
| `git remote add` | Adiciona um remote |
| `git remote set-url` | Altera a URL do remote |
| `git fetch --all` | Busca atualizações dos remotes |
| `git diff --staged` | Compara alterações preparadas |
| `git branch -vv` | Mostra branches e upstreams |
| `git log --graph --all` | Visualiza o histórico em grafo |
| `git push -u origin main` | Envia e configura upstream |
| `git pull --rebase` | Atualiza usando rebase |
| `git restore` | Restaura arquivos rastreados |

---

## 21. Resumo final

### Criar um projeto e publicar

```bash
git init
git branch -M main
git add .
git commit -m "Primeiro commit"
git remote add origin URL
git push -u origin main
```

### Atualizar o projeto

```bash
git fetch origin
git pull
```

### Salvar e enviar alterações

```bash
git status
git diff
git add .
git commit -m "Nova funcionalidade"
git push
```

### Consultar o histórico

```bash
git log --oneline --graph --all
```

**Regra principal:**

- `add` → prepara alterações.
- `commit` → salva no histórico local.
- `push` → envia ao GitHub.
- `fetch` → consulta e baixa atualizações remotas.
- `pull` → baixa e integra atualizações.
- `diff` → compara alterações.
- `remote` → gerencia conexões com outros repositórios.

**Lembre-se:** Git funciona sem internet. A conexão com o GitHub é necessária para operações remotas como push, fetch e pull.
