# Guia de Aprendizado: Git e GitHub

Este guia introdutório explica os comandos básicos do Git e apresenta fluxos práticos para aprender a trabalhar com Git e GitHub. Inclui também um passo a passo para resolver conflitos e um fluxo completo com Fork → Pull Request (PR) para uso em repositório didático.

---

## Sumário
- Visão geral rápida
- Comandos básicos (com exemplos)
  - `git status`
  - `git init`
  - `git clone`
  - `git add`
  - `git commit`
  - `git push`
  - `git pull`
- Como resolver conflitos (passo a passo)
- Fluxo completo com GitHub: Fork → PR
- Dicas rápidas e boas práticas
- Comandos úteis adicionais

---

## Visão geral rápida
Git é um sistema de controle de versão distribuído. Cada desenvolvedor possui uma cópia completa do histórico. GitHub é uma plataforma remota para hospedar repositórios Git, facilitar colaboração e gerenciar Pull Requests (PRs).

---

## Comandos básicos

Antes de tudo, configure seu nome e e‑mail (uma vez por máquina):
```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
```

### git status
O que faz: mostra o estado atual do repositório — branch atual, arquivos modificados, staged (prontos para commit) e arquivos não rastreados.
Quando usar: sempre antes de commitar ou enviar mudanças.
Exemplo:
```bash
git status
```
Saída típica:
```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   arquivo.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	novo_arquivo.txt
```

### git init
O que faz: cria um repositório Git local (diretório `.git`).
Quando usar: ao iniciar um projeto novo que ainda não é repositório.
Exemplo:
```bash
mkdir meu-projeto
cd meu-projeto
git init
echo "# Meu Projeto" > README.md
git add README.md
git commit -m "Inicializa repositório com README"
```

### git clone
O que faz: copia um repositório remoto para sua máquina (histórico completo).
Quando usar: para começar a trabalhar a partir de um repositório existente.
Exemplos:
```bash
# por HTTPS
git clone https://github.com/usuario/repo.git

# por SSH (requer chave SSH configurada)
git clone git@github.com:usuario/repo.git

# clonar em uma pasta com nome diferente
git clone https://github.com/usuario/repo.git minha-pasta
```

### git add
O que faz: adiciona alterações à staging area (index) — prepara para o commit.
Quando usar: depois de criar/modificar arquivos e antes de `git commit`.
Exemplos:
```bash
# adicionar um arquivo específico
git add arquivo.txt

# adicionar todos os arquivos modificados e novos
git add .

# adicionar alterações apenas de arquivos já rastreados
git add -u
```

### git commit
O que faz: registra no histórico um snapshot com as alterações staged.
Quando usar: para salvar um conjunto coerente de mudanças.
Exemplos:
```bash
# commit com mensagem curta
git commit -m "Corrige bug no cálculo de total"

# abrir o editor para escrever mensagem longa
git commit
```
Para alterar o último commit (quando ainda não foi enviado):
```bash
git commit --amend -m "Mensagem atualizada do último commit"
```

### git push
O que faz: envia commits locais para o repositório remoto (por exemplo, GitHub).
Quando usar: para publicar suas mudanças.
Exemplos:
```bash
# adicionar remote (apenas se ainda não existir)
git remote add origin https://github.com/usuario/repo.git

# enviar a branch main e configurar upstream
git push -u origin main

# push subsequente simples (quando upstream já está definido)
git push
```
Se houver rejeição (rejected) por estar atrás do remoto, faça `git pull` (ou `git pull --rebase`) primeiro e trate conflitos, se houver.

### git pull
O que faz: busca (fetch) do remoto e faz merge (por padrão) na branch atual. Equivale a `git fetch` + `git merge`.
Quando usar: antes de iniciar trabalho ou antes de `git push` quando remoto avançou.
Exemplos:
```bash
# buscar e mesclar alterações da branch main remota para a atual
git pull origin main

# buscar e rebasear suas mudanças sobre o remoto (histórico mais linear)
git pull --rebase origin main
```
Observação: `git pull` pode gerar conflitos — siga o passo a passo de resolução abaixo.

---

## Como resolver conflitos — passo a passo

Conflitos ocorrem quando você e outra pessoa modificaram a mesma área do mesmo arquivo. Aqui está um fluxo prático para resolver:

1. Recebe o conflito ao fazer `git pull`, `git merge` ou `git rebase`.
   Exemplo:
   ```bash
   git pull origin main
   # ou
   git merge feature-branch
   ```

2. O Git vai interromper a operação e mostrar arquivos em conflito. Verifique com:
   ```bash
   git status
   ```
   Saída mostrará os arquivos com conflito (marked as "both modified").

3. Abra o arquivo em conflito. Você verá marcações:
   ```
   <<<<<<< HEAD
   Seu conteúdo local
   =======
   Conteúdo vindo do remoto/outro branch
   >>>>>>> branch-remoto
   ```
   Edite o arquivo para escolher/integrar as linhas corretas e remova as marcações (`<<<<<<<`, `=======`, `>>>>>>>`).

4. Depois de editar e salvar, marque o arquivo como resolvido adicionando-o ao staging:
   ```bash
   git add arquivo-com-conflito.txt
   ```

5. Se você estava em um merge:
   - Faça commit do merge:
     ```bash
     git commit -m "Resolve conflito em arquivo-com-conflito.txt"
     ```
   Se estava em um rebase:
   - Continue o rebase:
     ```bash
     git rebase --continue
     ```
   Caso queira abortar a operação:
   - Abortar merge:
     ```bash
     git merge --abort
     ```
   - Abortar rebase:
     ```bash
     git rebase --abort
     ```

6. Verifique o histórico e teste seu projeto para garantir que tudo funciona:
   ```bash
   git log --oneline --graph --decorate -n 10
   ```

Dicas práticas:
- Resolva conflitos um arquivo por vez.
- Teste localmente após resolver antes de enviar (push).
- Se muitas alterações complexas: crie um branch temporário como backup antes de tentar resolver (ex.: `git branch backup-resolve`).

---

## Fluxo completo com GitHub: Fork → PR (para repositórios de aprendizado)

Este fluxo é ideal para contribuir em repositórios públicos (ex.: exercícios, tutoriais):

1. Fork (no GitHub)
   - No repositório original (upstream), clique em "Fork" para criar uma cópia no seu GitHub (seu usuário).
   - Resultado: `github.com/seu-usuario/repo` (fork).

2. Clonar seu fork localmente
   ```bash
   git clone https://github.com/seu-usuario/repo.git
   cd repo
   ```

3. Configurar o remote `upstream` para o repositório original (opcional, mas recomendado)
   ```bash
   git remote add upstream https://github.com/proprietario-original/repo.git
   git fetch upstream
   ```

4. Criar uma branch para sua mudança (não trabalhe direto em `main`)
   ```bash
   git switch -c minha-feature
   # ou (se git antigo)
   # git checkout -b minha-feature
   ```

5. Fazer mudanças, adicionar e commitar
   ```bash
   # editar arquivos...
   git add .
   git commit -m "Implementa exercício X: descrição curta"
   ```

6. Atualizar sua branch com mudanças do upstream (evitar divergências)
   - Buscar e rebasear na `main` do upstream:
     ```bash
     git fetch upstream
     git switch minha-feature
     git rebase upstream/main
     ```
     (ou `git merge upstream/main` se preferir merge)

   - Resolver conflitos se houver (veja seção de conflitos).

7. Enviar (push) sua branch para o seu fork
   ```bash
   git push -u origin minha-feature
   ```

8. Criar o Pull Request (PR) no GitHub
   - Vá ao seu fork no GitHub. Haverá um botão "Compare & pull request" — clique.
   - Escolha como base: `proprietario-original:main` e compare: `seu-usuario:minha-feature`.
   - Escreva título claro, descrição do que a mudança faz e testes realizados.
   - Envie o PR.

9. Revisão e interação
   - Mantenha seu branch atualizado se revisores pedirem mudanças: faça commits adicionais na mesma branch e `git push`.
   - Se o mantenedor atualizou a base (upstream/main), rebase/merge conforme necessário:
     ```bash
     git fetch upstream
     git rebase upstream/main
     git push --force-with-lease  # somente se rebaseou e entendeu o impacto
     ```
   - Evite `--force` sem `--force-with-lease` em colaboração pública.

10. Após aprovação
    - O mantenedor pode mesclar o PR. Se for você que vai mesclar (em projeto próprio), prefira `Merge` ou `Squash and merge` conforme política do projeto.

11. Sincronizar seu fork com o upstream (para futuros trabalhos)
    ```bash
    git fetch upstream
    git switch main
    git merge upstream/main
    git push origin main
    ```

Observações sobre conflitos no contexto de PR:
- Se houver conflitos apontados pelo GitHub ao abrir o PR, atualize localmente a branch com o upstream e resolva conflitos conforme o passo a passo de resolução; depois `push` para atualizar o PR.

---

## Dicas rápidas e boas práticas
- Faça commits pequenos e com mensagens claras (o que e por que).
- Trabalhe em branches por recurso/bug: `feature/x`, `fix/y`.
- Atualize sua branch com upstream regularmente para reduzir conflitos.
- Use `git pull --rebase` para um histórico linear quando apropriado.
- Não faça force push em branches compartilhadas sem coordenação.
- Escreva uma descrição completa ao abrir PRs (como testar, cenário, screenshots se necessário).

---

## Comandos úteis adicionais
```bash
# ver histórico de forma legível
git log --oneline --graph --decorate

# listar branches locais e remotas
git branch -a

# criar branch
git branch nova-branch

# trocar de branch
git switch outra-branch

# mesclar branch
git merge minha-feature

# redefinir working tree (descartar alterações locais não comitadas)
git restore arquivo.txt

# remover staging
git restore --staged arquivo.txt

# buscar do remoto sem merge
git fetch origin

# comparar diferenças
git diff origin/main..minha-feature
```

---

## Conclusão
Este arquivo contém um panorama prático para começar com Git e GitHub, resolver conflitos e contribuir via Fork → PR. Serve como material de apoio para um repositório de aprendizado.

Boa prática: adicione este arquivo ao seu repositório na raiz como `APRENDENDO_GIT_GITHUB.md` e atualize com comandos/políticas específicas do curso/projeto.
