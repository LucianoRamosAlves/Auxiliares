# Git Básico — Principais Comandos

## O que é Git?
Git é um sistema de controle de versão que permite acompanhar alterações e manter o histórico de um projeto.

## 1. git init
Inicializa um repositório Git na pasta atual.

git init

- Cria uma pasta oculta chamada `.git`.
- Permite começar a controlar as versões do projeto.

## 2. git status
Mostra o estado atual dos arquivos.

git status

- Identifica arquivos modificados.
- Mostra arquivos novos.
- Informa quais alterações estão prontas para commit.

## 3. git add
Adiciona alterações à área de preparação (staging).

git add .

- Adiciona todas as alterações.

git add arquivo.txt

- Adiciona um arquivo específico.

## 4. git commit
Registra as alterações preparadas no histórico.

git commit -m "Primeiro commit"

- Cria um registro das alterações.
- Cada commit possui um identificador único.

## 5. git log
Mostra o histórico de commits.

git log

- Exibe os commits realizados.
- Mostra autor, data e mensagem.

## 6. Outros comandos úteis

| Comando | Função |
|---------|--------|
| git branch | Lista as branches locais |
| git branch nome | Cria uma branch |
| git switch nome | Troca de branch |
| git clone URL | Clona um repositório |
| git diff | Mostra alterações não preparadas |
| git log --oneline | Mostra o histórico resumido |

## 7. Fluxo básico do Git

1. Criar ou abrir uma pasta.
2. Executar `git init`.
3. Criar ou modificar arquivos.
4. Executar `git status`.
5. Executar `git add .`.
6. Executar `git commit -m "Mensagem"`.
7. Executar `git log` para consultar o histórico.

## Resumo final

- `git init` → Inicia o repositório.
- `git status` → Verifica as alterações.
- `git add .` → Prepara as alterações.
- `git commit -m "mensagem"` → Registra as alterações.
- `git log` → Consulta o histórico.

**Importante:** Git e GitHub não são a mesma coisa.
Git controla versões localmente; GitHub hospeda repositórios na internet.