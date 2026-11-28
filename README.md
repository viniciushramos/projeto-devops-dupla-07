# Projeto DevOps: dupla-07

Site: https://viniciushramos.github.io/projeto-devops-dupla-07/ (status em /status.html)
Imagem: ghcr.io/viniciushramos/projeto-devops-dupla-07

## O fluxo completo
1. Fazemos o push do código (ou da tag vN) no GitHub.
2. O GitHub Actions roda os testes (pytest); se falharem, nada é publicado.
3. Com os testes aprovados, o site é publicado no Pages com o status.html.
4. Ao criar uma tag vN, o fluxo Publicar imagem constrói a imagem Docker e a envia ao ghcr.io.
5. Em qualquer máquina, docker pull e docker run da tag escolhida deixam o contêiner rodando na porta 5007.

## Linha do tempo
- v1: primeira imagem publicada, mostrando "versao":"v1" em /saude.
- v2: a porta foi trocada para 5008; os testes passaram, mas o contêiner deixou de responder na 5007.
- v3: o commit da v2 foi desfeito com git revert; a porta voltou a 5007 e /saude mostra "versao":"v3".

## O que os testes não pegaram
Os testes usam o cliente de teste do Flask, que não abre uma porta de rede, então a troca para a 5008 passou. Faltou verificar a aplicação rodando de verdade, por exemplo subindo o contêiner no fluxo e chamando /saude na porta 5007 antes de publicar a imagem.

## Quem fez o quê
Variação da dupla: nome dupla-07, mensagem "Olá da dupla 07", porta 5007.

Vinicius: criou a API Flask, o fluxo de testes (ci.yml) e o test_vinicius.py, a página de status com testes antes da publicação (pages.yml), verificou a visibilidade da imagem no ghcr.io, fez o git revert da v2 e publicou a v3.

Vitor: criou o Dockerfile e o workflow de publicação de imagens no GHCR, realizou o commit e publicou a primeira versão (v1). Em seguida, alterou a porta da aplicação de 5007 para 5008, publicou a v2 e testou o contêiner, identificando por meio do curl e dos logs que os testes automatizados não detectaram a falha. Executou o rollback operacional para a v1, restaurando o serviço, e posteriormente validou a versão v3, publicada após o git revert realizado por Vinicius, confirmando o funcionamento da aplicação na porta 5007.
