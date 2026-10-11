
# Git — Conflitos e Como Resolver

## 1. O que é um conflito no Git?

Um conflito acontece quando o Git não consegue combinar automaticamente alterações diferentes.

Isso pode ocorrer quando duas pessoas modificam o mesmo trecho de código em branches diferentes.

Também pode acontecer quando você tenta integrar alterações remotas às suas alterações locais.

### Exemplo

Imagine que duas pessoas estão trabalhando no arquivo `app.py`.

Na branch `main`:

```python
print("Bem-vindo ao sistema")
```

Na branch `login`:

```python
print("Bem-vindo ao login")
```

Ao tentar integrar as branches, o Git pode não saber qual alteração manter.

---

## 2. Quando os conflitos acontecem?

Os conflitos podem aparecer durante operações como:

- `git merge` — integração de branches.
- `git pull` — atualização do repositório local.
- `git rebase` — reaplicação de commits.
- `git cherry-pick` — aplicação de um commit específico.
- `git stash pop` — recuperação de alterações temporárias.

**Importante:** nem toda alteração no mesmo arquivo gera conflito. Muitas alterações podem ser combinadas automaticamente.

---

## 3. Como identificar um conflito?

Imagine que você está na branch `main` e deseja integrar a branch `login`.

```bash
git switch main
git merge login
```

O terminal pode apresentar:

```text
Auto-merging app.py
CONFLICT (content): Merge conflict in app.py
Automatic merge failed; fix conflicts and then commit the result.
```

Isso significa que o Git não conseguiu concluir a integração automaticamente.

### Verificar os arquivos em conflito

```bash
git status
```

Exemplo:

```text
Unmerged paths:
  both modified: app.py
```

O arquivo `app.py` precisa ser resolvido.

---

## 4. Entendendo os marcadores de conflito

Quando ocorre um conflito de conteúdo, o Git pode inserir marcadores diretamente no arquivo.

Exemplo:

```python
<<<<<<< HEAD
print("Bem-vindo ao sistema")
=======
print("Bem-vindo ao login")
>>>>>>> login
```

### O que significa cada marcador?

| Marcador | Significado |
|---|---|
| `<<<<<<< HEAD` | Início de uma versão |
| `=======` | Separação das versões |
| `>>>>>>> login` | Fim da outra versão |

Em um merge comum:

- `HEAD` representa a versão da branch atual.
- `login` representa a versão que está sendo integrada.

**Atenção:** em operações como rebase, o significado de "atual" e "recebida" pode ser diferente.

---

## 5. Como resolver manualmente?

Existem três possibilidades principais.

### Opção 1 — Manter a primeira versão

```python
print("Bem-vindo ao sistema")
```

### Opção 2 — Manter a segunda versão

```python
print("Bem-vindo ao login")
```

### Opção 3 — Combinar as duas versões

```python
print("Bem-vindo ao sistema")
print("Bem-vindo ao login")
```

Depois de escolher, remova todos os marcadores:

- `<<<<<<<`
- `=======`
- `>>>>>>>`

Salve o arquivo.

---

## 6. Finalizar a resolução do merge

Após corrigir o arquivo:

```bash
git add app.py
```

Depois:

```bash
git merge --continue
```

O Git concluirá o merge, podendo abrir o editor para confirmar a mensagem do commit.

Também é possível concluir um merge comum com:

```bash
git commit
```

### Verificar o resultado

```bash
git status
git log --oneline --graph --all
```

Se tudo estiver correto, você poderá enviar as alterações:

```bash
git push
```

---

## 7. Resolver conflitos pelo VS Code

O VS Code possui ferramentas visuais para resolver conflitos.

Ao abrir um arquivo em conflito, ele pode oferecer opções como:

- **Accept Current Change** — aceita a alteração atual.
- **Accept Incoming Change** — aceita a alteração recebida.
- **Accept Both Changes** — mantém ambas.
- **Compare Changes** — compara as versões.

Também existe o **Merge Editor**, que facilita a visualização das alterações.

### Passo a passo

1. Execute o merge pelo terminal.
2. Abra o arquivo em conflito no VS Code.
3. Compare as alterações.
4. Escolha ou edite a versão correta.
5. Salve o arquivo.
6. Execute `git add`.
7. Finalize com `git merge --continue`.

**Dica:** não escolha automaticamente "Accept Both". Às vezes, manter as duas versões pode quebrar o código.

---

## 8. Cancelar um merge

Se você não quiser continuar resolvendo os conflitos:

```bash
git merge --abort
```

Esse comando tenta retornar ao estado anterior ao merge.

**Atenção:** alterações locais preexistentes podem dificultar o cancelamento. Prefira começar operações com a área de trabalho limpa.

---

## 9. Conflitos durante git pull

Imagine que você está trabalhando localmente e outra pessoa enviou alterações ao GitHub.

Você executa:

```bash
git pull
```

Se o pull estiver configurado para usar merge e houver conflito, o Git interromperá a integração.

### Resolver

```bash
git status
```

Abra os arquivos em conflito, corrija e salve.

Depois:

```bash
git add .
git merge --continue
```

Por fim:

```bash
git push
```

### Cancelar

Se o pull iniciou um merge:

```bash
git merge --abort
```

Se iniciou um rebase:

```bash
git rebase --abort
```

O comando de cancelamento depende da operação em andamento.

---

## 10. Conflitos durante git rebase

O rebase reaplica commits sobre outra base.

Exemplo:

```bash
git switch login
git rebase main
```

Se ocorrer conflito, o Git interromperá o processo.

### Resolver

```bash
git status
```

Corrija os arquivos e execute:

```bash
git add .
git rebase --continue
```

Repita até terminar.

### Cancelar o rebase

```bash
git rebase --abort
```

### Pular o commit problemático

```bash
git rebase --skip
```

**Cuidado:** `--skip` ignora o commit atual da sequência de rebase. Use somente quando entender quais alterações serão perdidas.

---

## 11. Conflitos durante cherry-pick

Imagine que você deseja aplicar um commit de outra branch.

```bash
git cherry-pick abc1234
```

Se ocorrer conflito:

```bash
git status
```

Corrija os arquivos e execute:

```bash
git add .
git cherry-pick --continue
```

### Cancelar

```bash
git cherry-pick --abort
```

---

## 12. Conflitos durante git stash

Você guardou alterações temporárias:

```bash
git stash
```

Depois tentou recuperá-las:

```bash
git stash pop
```

Se houver conflito, será necessário resolvê-lo manualmente.

### Resolver

```bash
git status
```

Corrija os arquivos e execute:

```bash
git add .
```

Após um conflito, o stash normalmente é mantido para evitar perda de dados.

Verifique:

```bash
git stash list
```

Não execute `git stash drop` até confirmar que recuperou corretamente todas as alterações.

---

## 13. git merge --ours e --theirs

Durante um conflito de merge, você pode escolher uma versão inteira de um arquivo.

### Manter a versão da branch atual

```bash
git checkout --ours app.py
```

### Manter a versão da branch integrada

```bash
git checkout --theirs app.py
```

Depois:

```bash
git add app.py
git merge --continue
```

**Atenção:** esses comandos escolhem uma versão inteira do arquivo, podendo descartar alterações da outra versão.

Em rebase, o significado de `ours` e `theirs` pode ser contraintuitivo.

---

## 14. Conflito de exclusão de arquivo

Também existem conflitos quando uma pessoa exclui um arquivo e outra o modifica.

Exemplo:

```text
CONFLICT (modify/delete): app.py deleted in HEAD and modified in login.
```

### Manter o arquivo

Se o arquivo estiver presente na área de trabalho:

```bash
git add app.py
```

### Confirmar a exclusão

```bash
git rm app.py
```

Depois, finalize o merge.

---

## 15. Como evitar conflitos?

### 1. Atualize seu repositório

Antes de começar a programar:

```bash
git fetch origin
git pull
```

### 2. Trabalhe com branches

Crie branches para funcionalidades:

```bash
git switch -c tela-login
```

### 3. Faça commits pequenos

Evite acumular muitas alterações diferentes em um único commit.

### 4. Comunique alterações importantes

Quando várias pessoas trabalham no mesmo projeto, combine quem modificará cada parte.

### 5. Integre alterações frequentemente

Evite deixar uma branch muito tempo sem receber atualizações da principal.

**Importante:** essas práticas reduzem a probabilidade de conflitos, mas não garantem que eles nunca acontecerão.

---

## 16. Exemplo prático — Sistema de estacionamento

Imagine que você está desenvolvendo um sistema Flask.

Na branch `main`, o arquivo contém:

```python
def calcular_valor():
    return 10
```

Na branch `pagamento`, você alterou:

```python
def calcular_valor():
    return 15
```

Agora você executa:

```bash
git switch main
git merge pagamento
```

Se houver conflito, o Git pode mostrar:

```python
def calcular_valor():
<<<<<<< HEAD
    return 10
=======
    return 15
>>>>>>> pagamento
```

Você decide que o valor correto é 15.

Corrige:

```python
def calcular_valor():
    return 15
```

Salva o arquivo e executa:

```bash
git add app.py
git merge --continue
```

Pronto! O conflito foi resolvido.

---

## 17. Resumo dos comandos

| Comando | Função |
|---|---|
| `git status` | Identifica conflitos |
| `git diff` | Mostra diferenças |
| `git diff --check` | Verifica problemas como espaços incorretos |
| `git add arquivo` | Marca arquivo como resolvido |
| `git merge --continue` | Continua um merge |
| `git merge --abort` | Cancela um merge |
| `git rebase --continue` | Continua um rebase |
| `git rebase --abort` | Cancela um rebase |
| `git cherry-pick --continue` | Continua um cherry-pick |
| `git cherry-pick --abort` | Cancela um cherry-pick |
| `git checkout --ours` | Escolhe uma versão do arquivo |
| `git checkout --theirs` | Escolhe a outra versão |

---

## 18. Fluxo rápido de resolução

### Identificar

```bash
git status
```

### Corrigir

Abra o arquivo e resolva os conflitos manualmente ou pelo VS Code.

### Marcar como resolvido

```bash
git add arquivo.py
```

### Continuar

Para merge:

```bash
git merge --continue
```

Para rebase:

```bash
git rebase --continue
```

Para cherry-pick:

```bash
git cherry-pick --continue
```

### Conferir

```bash
git status
```

Execute também os testes do projeto antes de enviar as alterações.

---

## Resumo final

Um conflito não significa que o projeto foi perdido.

Significa que o Git precisa de uma decisão para combinar alterações incompatíveis.

O processo básico é:

1. Identificar o conflito com `git status`.
2. Abrir o arquivo afetado.
3. Comparar as versões.
4. Corrigir o código.
5. Remover os marcadores de conflito.
6. Salvar o arquivo.
7. Executar `git add`.
8. Continuar a operação interrompida.
9. Testar o projeto.

**Regra principal:** nunca resolva um conflito apenas para fazer o Git parar de reclamar. Verifique se o código final mantém o comportamento desejado.
