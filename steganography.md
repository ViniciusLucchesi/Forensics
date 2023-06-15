<div align="center">

# Steganography

![development_progress](https://img.shields.io/badge/development-progress-orange)
![programming_language](https://img.shields.io/badge/shell-script-informational)
![date](https://img.shields.io/badge/class-14/06/2023-informational)

See how steganography works based on practical examples.

All images and exemples used on this repository will be available on this [Google Drive](https://drive.google.com/drive/folders/1xWlrwM7W38h8E6JtuCbDOMazfARTOlms) folder.

<br />
<br />

</div>

## Disclaimer

Do not perform any of these actions on your everyday computer, be aware that some of the files contained in the Google Drive folder contain viruses.

All the commands described in the other repositories were executed in a Kali Linux ISO running in a virtual machine.

<br />

## Getting started

Instalar o **steghide**

```bash
sudo apt install steghide
```

Durante sua execusao ele pede uma senha
tentar a frase encontrada dentro da imagem

```bash
steghide extract -sf <nome_do_arquivo>.<extensao>
```

Criando um bruteforce em shell que irá analizar todos os arquivos de imagem no path especificado e retornará somente as frases válidas em um arquivo chamado `words.txt` (que deve ser criado anteriormente)

```bash
#!/bin/bash

path="/home/kali/Downloads/stego/Caso_Steg/imagens"

for file in "$path"/*; do
	if [ -f "$file" ]; then
		echo "[*] Analisando: $file"
		for word in `cat words.txt`; do
			steghide extract -sf $file -p $word > /dev/null 2>&1
			if [ $? = 0 ]; then
				echo "    [+] Encontrou algo!"
				echo ""
			fi
		done
	fi
done
```

tornando o arquivo shell criado acima em um executavel

```bash
chmod +x steg.sh
```

executando o arquivo recem criado

```bash
./steg.sh
```
