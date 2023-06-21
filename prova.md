<div align="center">

# Prova

![development_progress](https://img.shields.io/badge/Status-concluído-green) &nbsp;
![start_date](https://img.shields.io/badge/início-14/06/2023-informational) &nbsp;
![end_date](https://img.shields.io/badge/fim-21/06/2023-informational) &nbsp;

Encontre todas as senhas escondidas no arquivo **img_p1.dd**

O arquivo pode ser encontrado no [Google Drive](https://drive.google.com/drive/folders/1xWlrwM7W38h8E6JtuCbDOMazfARTOlms) disponibilizado pelo professor.

<br />

[Senhas encontradas](#senhas-encontradas) • [Buscando Senhas](#buscando-senhas) • [Recuperando arquivos](recuperacao_arquivos.md#recuperação-de-arquivos) • [Brute Force](#brute-force---steghide)

<br />

</div>

## Disclaimer

Não execute nenhuma dessas ações em seu computador de uso diário, esteja ciente de que alguns dos arquivos contidos na pasta do Google Drive contêm vírus.

Todos os comandos descritos nos outros repositórios foram executados em um Kali Linux ISO rodando em uma máquina virtual.

<br />

## Objetivo

Encontrar todas as senhas que estão escondidas dentro do arquivo **img_p1.dd**, que foram escondidas utilizando a técnica de esteganografia.

<br />

## Senhas encontradas

```text
1. panic
2. alma
3. seus_olhos
```

<br />

## Configurações iniciais

Primeiramente precisamos extrair todos os arquivos da imagem **img_p1.dd**, para isso vamos criar uma pasta chamada **analise** onde serão extraídos os arquivos.

```sh
mkdir analise
```

```sh
sudo mount -o ro,noexec,offset=$((512*2048)) img_p1.dd analise
```

<br />

## Buscando senhas

Dentro do nosso diretório `prova` vamos rodar o **grep** buscando pela palavra **senh** utilizando o comando `grep` de maneira recursiva sem ser case sensitive com a adição dos parâmetros `-ri`.

```sh
grep -ri "senh" .
```

```sh
OUTPUT

grep: ./output/ole/00006338.ole: binary file matches
grep: ./img_p1.dd: binary file matches
grep: ./analise/.Trash-0/files/olhe a paisagem.jpg: binary file matches
grep: ./analise/.Trash-0/files/eye.jpg: binary file matches
```

Como podemos ver existem alguns arquivos que contém a palavra **senh**, que estão na lixeira.

Então antes de mais nada vamos tentar recuperar esses e outros arquivos que estão na lixeira, para conseguirmos analisa-los.

<br />

## Recuperando arquivos da imagem

Para realizarmos a recuperação dos arquivos da imagem **img_p1.dd** sem poluir muito nossos resultados da prova iremos faze-lo em um [arquivo separado](recuperacao_arquivos.md#recuperação-de-arquivos).

<br />

## Brute Force - steghide

Para não perdermos tempo utilizando rodando o comando `steghide` arquivo por arquivo criaremos o script `steg.sh` que testará arquivo por arquivo com cada uma das senhas encontradas.

```sh
#!/bin/bash

path="/home/kali/Downloads/prova/imagens"

for file in "$path"/*; do
    if [ -f "$file" ]; then
        echo "[*] Analisando: $file"
        for word in `cat words.txt`; do
            steghide extract -sf $file -p $word > /dev/null 2>&1
            if [ $? -eq 0 ]; then
                echo "    [+] Encontrou algo!"
                echo "    [+] Chave: $word
                echo ""
            fi
        done
    fi
done
```

### Script steg.sh

Ao total somente esses 3 arquivos foram abertos através deste script shell, nos retornando **2 novas imagens** e **1 log de erro**, sendo uma das 2 novas imagens a **Ayishah.jpg** que nos devolveu o **log de erro**.

```sh
OUTPUT 

[*] Analisando: /home/kali/Downloads/prova/imagens/Ayishah.jpg
    [+] Encontrou algo!
    [+] Chave: panic

[*] Analisando: /home/kali/Downloads/prova/imagens/olhos.jpg
    [+] Encontrou algo!
    [+] Chave: seus_olhos

[*] Analisando: /home/kali/Downloads/prova/imagens/paisagem.jpg
    [+] Encontrou algo!
    [+] Chave: alma
```
