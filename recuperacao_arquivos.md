<div align="center">

# Recuperação de Arquivos

![development_progress](https://img.shields.io/badge/development-progress-orange)
![start_date](https://img.shields.io/badge/Start-20/06/2023-informational)
![end_date](https://img.shields.io/badge/End-21/06/2023-informational)

Os dados foram extraídos da sessão [Todos os arquivos da imagem](prova.md#todos-os-arquivos-da-imagem)

Este repositório tem o objetivo de organizar e recuperar quais arquivos ainda não foram recuperados.

<br />
<br />

</div>

## Configurações iniciais

Para armazenarmos esses novos arquivos recuperados vamos coloca-los em uma nova pasta chamada **recovered**.

```sh
mkdir recovered
```

<br />

## Arquivos encontrados

```sh
fls -apro 2048 img_p1.dd
```

## OUTPUT

### Arquivos repetidos

- [ ] 5:          carro1.jpg
- [ ] 7:          carro2.png
- [ ] 9:          carro3.jpg
- [ ] 11:         carro4.jpg
- [ ] 47:         Dennis Ritchie.jpg
- [ ] 57:         Klingon.jpg
- [ ] 19721:      Backup_viagem/olhos.jpg
- [ ] 6664:       .Trash-1000/info/BKP_Preferida.jpg.trashinfo
- [ ] 6670:       .Trash-1000/info/Burns sexy - Minha foto para o Linkedin.jpg.trashinfo
- [ ] 6674:       .Trash-1000/info/Controle_Clientes.xlsx.trashinfo
- [ ] 6677:       .Trash-1000/info/foto1.png.trashinfo
- [ ] 6680:       .Trash-1000/info/foto02.jpg.trashinfo
- [ ] 6683:       .Trash-1000/info/foto03.jpg.trashinfo
- [ ] 6686:       .Trash-1000/info/foto04.jpg.trashinfo
- [ ] 6689:       .Trash-1000/info/foto05.jpg.trashinfo
- [ ] 6692:       .Trash-1000/info/Lembrete.doc.trashinfo
- [ ] 394633:     .Trash-0/info/dia-da-toalha-macacos-espaciais.png.trashinfo
- [ ] 394636:     .Trash-0/info/Esfiha.jpg.trashinfo
- [ ] 394640:     .Trash-0/info/olhe a paisagem.jpg.trashinfo
- [ ] 394643:     .Trash-0/info/eye.jpg.trashinfo
- [ ] **[DEL]** 394646:   .Trash-0/info/eye.jpg.trashinfo.8UFTKZ
- [ ] **[DEL]** 6696:     .Trash-1000/info/Lembrete.doc.trashinfo.11FEVX
- [ ] **[DEL]** 50:       .carta.exe.swp
- [ ] **[DEL]** 53:       _ARTAE~1.SWX

## Arquivos novos

- [x] 19718:    Backup_viagem/eye.jpg        **[DEL]**
- [x] 19719:    Backup_viagem/_LHEAP~1.JPG   **[DEL]**
- [x] 19723:    Backup_viagem/paisagem.jpg   **[DEL]**
- [x] 52:       carta.exe                    **[DEL]**
- [x] 55:       Esfiha.jpg                   **[DEL]**
- [x] 6791:       .Trash-1000/files/BKP_Preferida.jpg
- [x] 6796:       .Trash-1000/files/Burns sexy - Minha foto para o Linkedin.jpg
- [x] 6799:       .Trash-1000/files/Controle_Clientes.xlsx
- [x] 6801:       .Trash-1000/files/foto1.png
- [x] 6803:       .Trash-1000/files/foto02.jpg
- [x] 6805:       .Trash-1000/files/foto03.jpg
- [x] 6807:       .Trash-1000/files/foto04.jpg
- [x] 6809:       .Trash-1000/files/foto05.jpg
- [x] 6811:       .Trash-1000/files/Lembrete.doc
- [x] 394760:     .Trash-0/files/dia-da-toalha-macacos-espaciais.png
- [x] 394762:     .Trash-0/files/Esfiha.jpg
- [x] 394765:     .Trash-0/files/olhe a paisagem.jpg
- [x] 394767:     .Trash-0/files/eye.jpg

<br />

## Recuperando os arquivos

Os únicos arquivos que serão recuperados serão os arquivos que julgamos serem novos.

```sh
# Backup_viagem
icat -o 2048 ../img_p1.dd 19718 > eye.jpg
icat -o 2048 ../img_p1.dd 19719 > _LHEAP~1.JPG
icat -o 2048 ../img_p1.dd 19723 > paisagem.jpg

# Normal directory
icat -o 2048 ../img_p1.dd 52 > carta.exe
icat -o 2048 ../img_p1.dd 55 > Esfiha.jpg

# Trash - 1000
icat -o 2048 ../../../img_p1.dd 6791 > BKP_Preferida.jpg
icat -o 2048 ../../../img_p1.dd 6796 > "Burns sexy - Minha foto para o Linkedin.jpg"
icat -o 2048 ../../../img_p1.dd 6799 > Controle_Clientes.xlsx
icat -o 2048 ../../../img_p1.dd 6801 > foto1.png
icat -o 2048 ../../../img_p1.dd 6803 > foto02.jpg
icat -o 2048 ../../../img_p1.dd 6805 > foto03.jpg
icat -o 2048 ../../../img_p1.dd 6807 > foto04.jpg
icat -o 2048 ../../../img_p1.dd 6809 > foto05.jpg
icat -o 2048 ../../../img_p1.dd 6811 > Lembrete.doc

# Trash - 0
icat -o 2048 ../../../img_p1.dd 394760 > dia-da-toalha-macacos-espaciais.png
icat -o 2048 ../../../img_p1.dd 394762 > Esfiha.jpg
icat -o 2048 ../../../img_p1.dd 394765 > "olhe a paisagem.jpg"
icat -o 2048 ../../../img_p1.dd 394767 > eye.jpg

```

<br />

## Analise dos dados recuperados

#### Backup_viagem/_LHEAP~1.JPG

```text
senha: panic

OBS: Senha escrita pelo Misha.
```

#### carta.exe

Este arquivo não é um executável, na realidade ele é um `ASCII text` que contém um texto com pontuações fora do comum, o que se encaixa em um `SPAM mimic`.

Link: [spammimic.com](https://www.spammimic.com/decode32.shtml)

```text
senha: alma

OBS: Foi utilizado a versão antiga do spammimic.com para conseguirmos decodificar a mensagem
```

#### trash/0/ "olhe a paisagem.jpg"

```text
senha: panic

OBS: Senha escrita pelo Misha.
```

#### trash/0/ eye.jpg

```text
senha: seus_olhos

OBS: Senha escrita pelo Misha.
```

<br />

#### Backup_viagem/paisagem.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### Esfiha.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### BKP_Preferida.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### BKP_Preferida.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### "Burns sexy - Minha foto para o Linkedin.jpg"

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### foto02.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### foto03.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### foto04.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### foto05.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### trash/0/ dia-da-toalha-macacos-espaciais.png

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

#### trash/0/ Esfiha.jpg

```text
senha: ????

OBS: Desbloqueio do arquivo pelo comando steghide (não temos a palavra secreta).
```

<br />

#### Controle_Clientes.xlsx

Se trata de uma planilha de Excel criada por Eduardo Montes - <eduardo@escritoriodeprojetos.com.br>

```text
senha: Nenhuma

OBS: O arquivo está vazio.
```

#### foto1.png

```text
senha: Nenhuma

OBS: O arquivo está vazio.
```

#### Lembrete.doc

```text
senha: Nenhuma

OBS: O arquivo está vazio.
```
