# ERREI E AGORA :interrobang: 

**O [Git](https://git-scm.com/) é uma ferramenta incrivelmente poderosa de controle de versões de software utilizada em projetos grandes ou pequenos. Inicialmente pode parecer bastante intimidador aprender todos os comandos do Git, principalmente quando cometemos um erro e somos tomados pelo medo de piorar a situação e perdermos o código escrito.**

A seguir, vamos explicar como corrigir os erros mais comuns do Git.

## Fiz algo errado no meu último commit, mas não dei push

Em situações em que você precisa corrigir seu último commit (antes de executar push para o remote) o comando `git commit --amend` após atualizar o arquivo vai te ajudar.

### Escrevi a mensagem de commit errada.

Se você precisa corrigir a mensagem de commit basta rodar o comando abaixo com a nova mensagem:

```
git commit --amend -m "Nova mensagem de commit"
```

### Esqueci de adicionar um arquivo ao commit

Se você precisa adicionar um arquivo que ficou fora do commit ou se precisou fazer alguma alteração em um arquivo:

```
git add arquivo-esquecido.md
git commit --amend
```

Note que, se você já fez o push para o seu repositório remoto, será necessário sobrescrever o commit refazendo o `push` com a opção `--force`. Tome cuidado com essa opção porque ela sobrescreve seu commit no remote.

## Preciso desfazer algo do último commit

Às vezes precisamos desfazer alguma modificação que fizemos. Para isso o `git --reset` pode nos ajudar.

### Adicionei um arquivo que não queria commitar

Se você não fez o commit basta resetar o arquivo com:

```
git reset -- caminho/do/arquivo.jpg
```

Um detalhe interessante: é sempre bom adicionar `--` antes do caminho para indicar que é um arquivo e não o nome de uma branch. Caso você tenha uma branch com o mesmo nome do arquivo, esse comando pode acabar em resultados indesejados.

Se você já fez o commit será necessário resetar o commit, depois remover o arquivo e fazer um novo commit:

```
git reset --soft HEAD~1
rm caminho/do/arquivo.jpg
git commit -m "Removendo arquivo"
```

Note que, nesse caso, estamos removendo o arquivo do nosso computador. Mas existem situações em que não queremos remover o arquivo local, que será descrito no tópico a seguir.

### Fiz commit de uma pasta ou arquivo, mas quero que o Git o ignore

Nesse caso, queremos que o arquivo ou pasta seja removido do repositório remoto e que ele pare de ser observado pelo Git. Você pode seguir os seguintes passos:

```
git reset pasta_para_ser_ignorada/
echo "pasta_para_ser_ignorada" >> .gitignore
git add .gitignore
git rm -r pasta_para_ser_ignorada/
git commit -m "Removendo pasta do git"
```

Essa sequência de comandos é um pouco mais complicada. Primeiro usamos o comando `reset` na pasta que precisa ser removida do Git. Depois disso, incluímos essa pasta no arquivo `.gitignore` para que o Git passe a ignorar as modificações que ocorram nela e adicionamos o arquivo ao commit. Em seguida podemos remover a pasta do Git, mas mantendo a cópia local, com o comando `git rm -r`. Finalmente executamos um novo commit para registrar as ações realizadas.

### Gostaria de desfazer as alterações realizadas no código

Se você quer voltar seu código inteiro para o estado de algum commit anterior, mas quer manter as adições atuais, você pode usar:

```
git reset --soft HEAD~1
# ou
git reset --soft ed51df2 
```

No primeiro caso, o número indica a quantidade de commits que você quer voltar atrás. No segundo exemplo, entra o código exato do commit ao qual você quer retornar.

Se você não quer manter as alterações atuais do código, você pode usar `--hard` no lugar de `--soft`. Mas tome cuidado, com essa opção o código será perdido.

Também existe uma outra opção, caso você queria desfazer alterações em arquivos específicos. Por exemplo, digamos que você tenha modificado um arquivo que não deveria e removido outro sem querer, mas quer manter todas as outras modificações. Se você ainda não fez o commit, basta usar o comando `checkout`:

```
git checkout HEAD -- arquivo_deletado.txt arquivo_modificado.txt
```

Dessa maneira ambos arquivos retornam ao estado do commit anterior.

### O nome da branch que eu criei está errado

Se você criou uma branch e percebeu que ela está com o nome incorreto, pode usar:

```
git branch -m branch-errada branch-correta
```

Caso você já tenha feito push dessa branch, será necessário apagá-la no remote também:

```
git push origin --delete branch-errada
git push origin branch-correta
```

[Fonte:](https://www.campuscode.com.br/conteudos/erros-mais-comuns-com-git-e-como-corrigi-los)