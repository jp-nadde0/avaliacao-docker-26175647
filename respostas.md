# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome:
Matrícula:
Usuário do GitHub:
Usuário do Docker Hub:

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
R: Usei `nginx:1.27-alpine` como imagem base. Tamanho final: [preencha com o valor da coluna SIZE após reconstruir a imagem].

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.
R: O Nginx procura os arquivos do site em /usr/share/nginx/html. Usei o comando docker run --rm --entrypoint sh portal-nginx -c "ls -l /usr/share/nginx/html/index.html".

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.

4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | `COPY pagina/ .` | A pasta `pagina/` não existe; os arquivos estão em `site/`. | O build falhou com `"/pagina": not found`. | Copiei `site/` para `/usr/share/nginx/html/`. |
| 2 | `WORKDIR /usr/share/nginx` | Não era a pasta padrão em que o Nginx serve o site. | Os arquivos não ficariam no diretório padrão do site. | Removi o `WORKDIR` e defini o destino do `COPY` como `/usr/share/nginx/html/`. |
| 3 | `CMD ["nginx"]` | O Nginx iniciava em background, em vez de permanecer em primeiro plano no container. | O container poderia encerrar logo após iniciar. | Removi o `CMD` personalizado e mantive o comando padrão da imagem oficial. |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
R: Em `-p HOST:CONTAINER`, o primeiro número é a porta do host e o segundo é a porta do container. Assim, `-p 7042:80` publica a porta 80 do container na porta 7042 do host; `-p 80:7042` publica a porta 7042 do container na porta 80 do host. A porta do container é o segundo número.

## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.
R: Primeiro, construir a imagem local de manutenção:
```bash
docker build -t manutencao:26175647 ./manutencao
```

Depois, iniciar os dois serviços:
```bash
docker run -d --name portal -p 8047:80 --restart unless-stopped SEU_USUARIO_DOCKERHUB/viaserra-portal:1.0-26175647
docker run -d --name manutencao -p 7047:80 --restart unless-stopped manutencao:26175647
```

8. Qual comando derruba os dois containers de uma vez?
R: `docker compose down`

## Verificador

9. Código de conclusão impresso pelo verificador:
R: jajan@NoteDoJota MINGW64 ~/Downloads/avaliacao-docker-viaserra (avaliacao-docker-26175647.git/main)
$ bash scripts/verificar.sh
================================================================
 Verificador · Avaliação Prática de Docker · Turma C
================================================================
 Matrícula 26175647 · portal 8047 · manutenção 7047

A. Arquivos e Git
[ OK ] A1 portal/Dockerfile segue os requisitos
[ OK ] A2 .env fora do Git e .env.example versionado
[ OK ] A3 4+ commits e remoto no GitHub (encontrados: 7)
[ OK ] A4 imagem jpnadde0/viaserra-portal:1.0-26175647 pública no Docker Hub

B. docker compose
[ OK ] B1 serviços portal e manutencao em execução
[ OK ] B2 portal roda a imagem publicada
[ OK ] B3 portas: portal em 8047 e manutenção em 7047

C. Conteúdo
[ OK ] C1 portal mostra seu nome e sua matrícula
[ OK ] C2 página de manutenção servindo o aviso "Voltamos em breve"

================================================================
 Resultado: 9/9 verificações
 Código de conclusão: VIASERRA-26175647-954B6611
```
(preencha depois que todas as verificações passarem)
```
