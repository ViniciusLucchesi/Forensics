<div align="center">

# Prova

![development_progress](https://img.shields.io/badge/development-progress-orange)
![start_date](https://img.shields.io/badge/Start-14/06/2023-informational)
![end_date](https://img.shields.io/badge/End-21/06/2023-informational)

Extract all information aboud **img_p1.dd**

All images and exemples used on this repository will be available on this [Google Drive](https://drive.google.com/drive/folders/1xWlrwM7W38h8E6JtuCbDOMazfARTOlms) folder.

<br />
<br />

</div>

## Objectives

- Recovered files
- Found passwords
- Final result

<br />

## Senhas encontradas

```text
seus_olhos
```

<br />

## Configurações iniciais

Primeiramente precisamos extrair todos os arquivos da imagem **img_p1.dd**.

```sh
mkdir analise
```

```sh
sudo mount -o ro,noexec,offset=$((512*2048)) img_p1.dd analise
```

<br />

## Recuperando arquivos da imagem

Para realizarmos a recuperação dos arquivos da imagem **img_p1.dd** sem poluir muito nossos resultados da prova iremos faze-lo em um [arquivo separado](recuperacao_arquivos.md#recuperação-de-arquivos).

<br />

## Buscando senhas

Dentro do nosso diretório `prova` vamos rodar o **grep** buscando pela palavra **senh** de maneira recursiva e sendo case sensitive.

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

### steg.sh

Na primeira vez rodando nosso Shell script obtivemos o seguinte resultado, retornando a imagem `ite.jpg`.

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
