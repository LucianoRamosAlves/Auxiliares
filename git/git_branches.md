
# Git — Branch e Checkout

## 1. O que é uma branch?

Uma branch é uma linha de desenvolvimento do projeto.

Ela permite desenvolver funcionalidades ou corrigir problemas sem alterar diretamente a branch principal.

Exemplo:
- `main` → versão principal
- `login` → desenvolvimento do login
- `cadastro` → desenvolvimento do cadastro

## 2. git branch

Lista as branches locais do repositório.

```bash
git branch
```

O asterisco (`*`) indica a branch atual.

## 3. Criar uma branch

```bash
git branch login
```

Cria a branch `login`, mas não muda automaticamente para ela.

## 4. git checkout

Troca para outra branch existente.

```bash
git checkout login
```

## 5. Criar e entrar em uma branch

```bash
git checkout -b cadastro
```

Cria a branch `cadastro` e muda para ela imediatamente.

## 6. git switch

Alternativa moderna para trocar de branch.

```bash
git switch main
```

Criar e entrar em uma nova branch:

```bash
git switch -c pagamentos
```

## 7. git merge

Integra alterações de outra branch à branch atual.

```bash
git switch main
git merge login
```

Isso incorpora as alterações da branch `login` à `main`.

## 8. Excluir uma branch

```bash
git branch -d login
```

Exclui a branch local se suas alterações já estiverem integradas.

Para forçar a exclusão:

```bash
git branch -D login
```

Atenção: a exclusão forçada pode descartar commits não integrados.

## 9. Outros comandos úteis

| Comando | Função |
|---------|--------|
| `git branch` | Lista branches locais |
| `git branch -a` | Lista branches locais e remotas conhecidas |
| `git branch nome` | Cria uma branch |
| `git checkout nome` | Troca de branch |
| `git checkout -b nome` | Cria e troca de branch |
| `git switch nome` | Troca de branch |
| `git switch -c nome` | Cria e troca de branch |
| `git merge nome` | Integra outra branch |
| `git branch -d nome` | Exclui branch local integrada |

## 10. Exemplo prático

Imagine que você está desenvolvendo um sistema de estacionamento.

### Criar uma branch para a tela de entrada

```bash
git switch -c tela-entrada
```

### Desenvolver e salvar as alterações

```bash
git add .
git commit -m "Cria tela de entrada"
```

### Voltar para a principal

```bash
git switch main
```

### Integrar a funcionalidade

```bash
git merge tela-entrada
```

### Excluir a branch após integrar

```bash
git branch -d tela-entrada
```

## Resumo final

- `git branch` → Lista ou cria branches.
- `git checkout` → Troca de branch e também pode restaurar arquivos.
- `git checkout -b` → Cria e troca de branch.
- `git switch` → Troca de branch de forma mais específica.
- `git switch -c` → Cria e troca de branch.
- `git merge` → Integra alterações.
- `git branch -d` → Exclui uma branch local integrada.

**Importante:** Criar uma branch não cria uma cópia independente de toda a pasta. O Git utiliza referências para organizar diferentes linhas de desenvolvimento.

**Dica:** Prefira `git switch` para trocar de branch e `git restore` para restaurar arquivos. O `git checkout` continua válido, mas possui múltiplas funções.
  