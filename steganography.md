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

## Tools

```sh
sudo apt install steghide
```

```sh
sudo apt install foremost
```

<br />

## Configurações iniciais

install **ctf_stego.png** and **Caso_Steg.zip** from [Google Drive](https://drive.google.com/drive/folders/1xWlrwM7W38h8E6JtuCbDOMazfARTOlms) folder.


Let's create a new folder to maintain our virtual environment cleaner, and move to there our previous files downloaded.

```sh
~/Downloads: mkdir stego
```

```sh
~/Downloads: mv ~/Downloads/ctf_stego.png ~/Downloads/stego/ctf_stego.png
```

```sh
~/Downloads: mv ~/Downloads/Caso_Steg.zip ~/Downloads/stego/Caso_Steg.zip
```

Extract the `zip` file and inside the extracted folder create a new folder called `analise`.

```sh
~/Downloads/stego/Caso_Steg: mkdir analise
```

<br />

## Getting started

- [ctf_stego.png](#ctf_stegopng)
- [Caso_Steg.zip](#caso_stegzip)

<br />


## ctf_stego.png

Primeiramente vamos analisar se esse nosso PNG é realmente um PNG. Para isso podemos rodar o comando abaixo.

```sh
file ctf_stego.png
```

```text
OUTPUT

ctf_stego.png: PNG image data, 1024 x 1024, 8-bit/color RGBA, non-interlaced
```

Esse retorno nos mostra que ele é realmente um arquivo PNG então vamos tentar outra estratégia.

Vamos olhar os metadados da imagem utilizando o `exiftool`.

```sh
exiftool ctf_stego.png
```

```text
OUTPUT

ExifTool Version Number         : 12.57
File Name                       : ctf_stego.png
Directory                       : .
File Size                       : 613 kB
File Modification Date/Time     : 2023:06:14 13:55:26-04:00
File Access Date/Time           : 2023:06:19 12:11:31-04:00
File Inode Change Date/Time     : 2023:06:19 12:11:19-04:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 1024
Image Height                    : 1024
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
SRGB Rendering                  : Perceptual
Image Size                      : 1024x1024
Megapixels                      : 1.0
```

Segundo os metadados extraídos da imagem, não há nenhuma informação estranha que poderiamos utilizar aqui.

Vamos tentar utilizar a busca por texto através do `strings`

```sh
strings -a ctf_stego.png | less
```

Mas novamente não foi possível encontrar nada de incomum, o que nos indica que ela não sofreu alteração de string, nem sobreposição.

Vamos então acessar o site [Forencically](https://29a.ch/) para buscar por píxels alterados.

![ctf_stego normal](/img/stego/ctf_stego%20-%20normal.png)

Ao passar a imagem acima e aplicar o filtro **Luminance Gradient** abaixo podemos ver que a mensagem `ALQ{stego_easy}` é exibida em relevo no canto superior direito da imagem.

![ctf_stego filtrado](/img/stego/ctf_stego%20-%20luminace%20Gradient.png)

Com isso concluímos esta imagem após encontrar a mensagem `ALQ{stego_easy}` escondida na imagem.

<br />


## Caso_Steg.zip

Dentro do nosso arquivo `ZIP` nós temos dois outros arquivos

- [paisagem.jpg](#paisagemjpg)
- [img_zen.dd](#img_zendd)

### paisagem.jpg

Se tentarmos analizar o arquivo `paisagem.jpg` utilizando-se da mesma técnica feita no arquivo `ctf_stego.png` não conseguiremos caminhar muito, ja que o arquivo ``paisagem.jpg` contém muito mais pixels do que a imagem anteriormente analizada.

Olhando os metadados desse arquivo podemos notar coisas estranhas, tais como o **Encoding Process** e o **Y Cb Cr Sub Sampling**, mas ainda sim há a possibilidade disse ser algo comum para quem tem maiores conhecimentos em imagens.

```sh
exiftool paisagem.jpg
```

```text
OUTPUT 

ExifTool Version Number         : 12.57
File Name                       : paisagem.jpg
Directory                       : .
File Size                       : 11 MB
File Modification Date/Time     : 2018:06:21 17:34:51-04:00
File Access Date/Time           : 2023:06:19 12:28:28-04:00
File Inode Change Date/Time     : 2023:06:19 12:11:43-04:00
File Permissions                : -rwxr-xr-x
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Image Width                     : 5120
Image Height                    : 3200
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 5120x3200
Megapixels                      : 16.4

```

Rodando o comando abaixo para ver as **strings** da imagem podemos notar que nos foi retornado um arquivo muito maior do que o anterior, não compensando ficar buscando por uma informação que nem sabemos como se parece.

```sh
strings -a paisagem.jpg
```

Como o arquivo é muito grande podemos buscar somente pelas 5 primeiras linhas para analisarmos.

```sh
head -n5 paisagem.jpg
```

```text
OUTPUT

����JFIF��C��C��
               ���

���}!1AQa"q2��#B��R��$3br�
▒▒%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz���������������������������������������������������������������������������

���w!1AQaq"2B����       #3R�br�
$4�%�▒▒&'()*56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz��������������������������������������������������������������������������
               ?�p���쒪����ęVv�DTN�
                                   G�j�Ri�S�뛾��1��[|`��`[y�۳��Ué�~L��W����*�
                                                                             5��"�G GG1�$�Ѽ���b����z

```

Como podemos ver, não da para associar nenhuma das 5 primeiras linhas a algo conhecido.

Neste caso vamos buscar rodapé do arquivo para ver o que podemos encontrar.

```sh
tail paisagem.jpg
```

```text
OUTPUT

(��
�ɇ����i�.g-�X����p�F}�<V��S)5i�d��kb]   y�b���Ģ�ڻc�G�+X�W���N*�So����|˻�r�ݍ)��!ѭ�M2XY7
                                                                                      ����T0�p�ߊ��
                                                                                                  ������$ȸ�$M����zǨ>���:bԌ���0p����Vu#uq�$�3+����6�0f�l��o2p��G*GJҚטƪ�\�v������R
�9n?.kъ�lsS�6��\%����dd3r0O_J����8�*��g���qp��
d�`��%�c[��b�A�1�
��88� �m�ɡ�&�)▒�ï�\�N��ɱ�=�?��(J(��
(P0��%�^�(���@:g�._?�ڮow�R��c�g���jf�@P@P@P@P@�����"���=�\����sHū;�~�g��fO#r���
                                                                               ����M\�n4�;8V��Cqj�CLX1�q�Vd�b?▒w!�ue����!�����{�as��AF�dm��A8 ����E�U�Ѣ_��v~e#�~~���S)(�%Q�fj�\K��W���F�L`�[�|�,�H�5p}Q���3=����x;DMJ�Z�K�fU��2�/��▒�F�mS�ة˚��U��~,ծ��:��9�;xeh��?*F�6����m*�j�K�������ލ!fv��X�q��5
�Z����#�r>e$�(��ǄrAg<c��ƨE�fT!X��O�娔�vB���7A3;Q�B9���;;�R�$ >�2s����s�Ga�X��
                                                   =��kUr���;��~a���s�n&ܪ���y�S��D
                                                                                  �y����Ȗ�# ��PzX�<����?_|��j���9�w��8>���N�m��e+>�1�s�ۯ=�i���ダ� ������˨
                                            !㎇�J�Q     ڡ97gЙ�.O��z�!S�c�׌b��m
                                                                              "(��ш�0�z��6�     3��e����3��-y�A�n�k(o#����5�@�T�8烜z�4���R��"0�@▒�`�H��՘{���3�m�-+Z���9���9��Q��SZбn��Frq�3w�M������\���T]��,�+#��2y稨�Kft�)��
���<��R6����k
             �B��_j�dP6�sA�v�d�O(I"Y
�n���9��gLР�▒a������ɸhە�89�~t�pܩT����NMZ�ӹWD    �����aV3�硧b\��m����fA������g�+ޙ�we▒I�?61��yrnS)�����sd�#*@������)jH▒_�?
�6���wq�ϧ�.a�%a▒��*C`f��=ɠ��9�f�_�E];^����Z�=�����2V������p�%+�
                                                               ��T�z��'��zg���M���@rq��3Q��qܒI�D�6����=���;j��(VR�}OL��h�đ$gﳯ��Ri���t���                                                                                                             

```

Como pode ser observado há caracteres japonêses espalhados pelo rodapé da imagem, o que nos indica que existe algo lá.

Mas para conseguirmos buscar o que existe lá vamos utilizar o comando `steghide`, instalado anteriormente na sessão [Tools](#tools), para extrair os arquivos secretos contidos na imagem passada.

```sh
steghide extract -sf paisagem.jpg
```

```sh
OUTPUT

Enter passphrase:
```

Ao rodar o comando ele nos pede uma senha de criptografia, que será utilizada para abrir o arquivo secreto dentro da imagem. No entando mesmo utilizando a frase `esvazi3_@_x1cara` a mensagem abaixo é exibida.

```sh
steghide: could not extract any data with that passphrase!
```

O que nos indica que essa não é a senha correta, ou que essa não é a imagem correta para a senha que encontramos, já que dentro da imagem `img_zen.dd` econtramos várias outras imagens.

Vamos facilitar a verificação de cada um dos arquivos de imagens movendo as imagens encontradas na pasta **xml** dentro da pasta **analise** para uma pasta chamada **imagens**.

Após alterarmos o diretório das imagens vamos criar um arquivo **Shell script** chamado `steg.sh`, que faça a verificação por nós.

```bash
#!/bin/bash

path="/home/kali/Downloads/stego/Caso_Steg/imagens"

for file in "$path"/*; do
	if [ -f "$file" ]; then
		echo "[*] Analisando: $file"
		for word in `cat words.txt`; do
			steghide extract -sf $file -p $word > /dev/null 2>&1
			if [ $? -eq 0 ]; then
				echo "    [+] Encontrou algo!"
				echo ""
			fi
		done
	fi
done
```

Com o arquivo criado vamos torna-lo um executável através do comando abaixo

```sh
chmod +x steg.sh
```

Agora basta executar nosso arquivo para analizarmos todos os arquivos da pasta.

```sh
./steg.sh
```

```text
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/all.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/baba.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/bear.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/budismo.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/flow.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/xamnism.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/zen.jpg
    [+] Encontrou algo!
```

Como mostra o retorno no terminal foi encontrado no arquivo `zen.jpg` algo. Se olharmos em nosso diretório poderemos ver que foi extraído do `zen.jpg` o arquivo `parabola`.

Vamos verificar sua extensão utilizando o comando `file`.

```sh
file parabola
```

```text
OUTPUT

parabola: Unicode text, UTF-8 text
```

Agora que sabemos que se trata de um arquivo de texto vamos ler seu conteúdo com o comando `cat`.

```sh
cat parabola
```

```text
OUTPUT

Um professor subiu uma montanha em busca de um mestre zen, para ter suas perguntas respondidas.

Chegando na casa do mestre, ele pergunta:

- Mestre onde está Deus ? O sr conhece ele ?
- Para onde vamos ... ?
- ... De onde viemos ?
- Como chegar lá ?
...

Então o mestre disse somente:
- Sente-se e tome um chá para encontrar suas respostas.

O professor irritado só não se levantou para ir embora, pois estava frio e um chá quente o ajudaria, entõ ele aceitou.

O mestre começou a encher a xicara... a mestra transbordou e começou a encher o pires, o professor indagou eufórico:
- Pare, está louco ? Irá derramar e perder o chá!

Disse o mestre:
- Então percebeu que somente uma gota a mais e o chá se perderia para fora do pires ? Bom é assim que a sua mente está.

- Sua mente borbulha de perguntas, enche e transborda, logo não há espaço para obter as respostas.
- Vá embora e esvazie a sua xicara, crie espaço para as resposta, deixa que as perguntas venham e vão, mas mentenha o espaço vazio.

O professor um pouco mais calmo e surpreso disse:
- Entendo, farei isto.
- Mas antes me diga mestre, o sr.acreita em  Deus ? Onde ele está ?

O mestre:
- Não, eu não acredito.

O professor:
- Como não ?

O mestre:
- Não preciso acreditar. Eu o conheço, eu o vejo e falo com ele.
- Não acredite, conheça-o, busque-o.
- Ele está em todo lugar, no chá, na água, no ar, nos que nascem, nos que seguem, em mim , em você...
- Vá e esvazie a mente para que ela possa se expandir.
- Namstê!

  2. xamanism
```

Aqui podemos notar que há uma segunda palavra secreta `xamanism`, então vamos adiciona-la em nossa word list.

Após adicionar esse nova palavra em nossa **word list** vamos rodar o comando novamente e ver o que mais ele encontra.

```sh
./steg.sh
```

```text
OUTPUT

[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/all.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/baba.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/bear.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/budismo.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/flow.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/xamnism.jpg
    [+] Encontrou algo!

[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/zen.jpg
    [+] Encontrou algo!
```

Dessa vez encontramos algo no arquivo `xamnism.jpg`, que nos retornou o arquivo `open.jpf`.

Rodando o comando `file` para verificar o tipo de arquivo nos deparamos novamente com um **ASCII text**.

Ao utilizar os comandos listados abaixo nos deparamos com o mesmo resultado.

```sh
head -n5 open.jpg
```

```sh
tial open.jpg
```

```text
OUTPUT

dac0fd1ad30d43fc353339239ac62dc4
```

Podemos ver que se trata de um **HASH**, mas de qual?? Podemos utilizar o comando `hashid` para obtermos uma lista de possibilidades.

```sh
hashid dac0fd1ad30d43fc353339239ac62dc4
```

```text
OUTPUT

Analyzing 'dac0fd1ad30d43fc353339239ac62dc4'
[+] MD2 
[+] MD5 
[+] MD4 
[+] Double MD5 
[+] LM 
[+] RIPEMD-128 
[+] Haval-128 
[+] Tiger-128 
[+] Skein-256(128) 
[+] Skein-512(128) 
[+] Lotus Notes/Domino 5 
[+] Skype 
[+] Snefru-128 
[+] NTLM 
[+] Domain Cached Credentials 
[+] Domain Cached Credentials 2 
[+] DNSSEC(NSEC3) 
[+] RAdmin v2.x
```

Utilizando o site [md5online](https://www.md5online.org/md5-decrypt.html) passamos o hash encontrado o que nos retornou outro **HASH MD5**, que ao passar novamente pela descriptografia nos retornou a palavra **one_mind**.

Vamos adiciona-la ao nosso arquivo de palavras e fazer todo o processo novamente.

```sh
./steg.sh
```

```text
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/all.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/baba.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/bear.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/budismo.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/flow.jpg
[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/xamnism.jpg
    [+] Encontrou algo!

[*] Analisando: /home/kali/Downloads/stego/Caso_Steg/imagens/zen.jpg
    [+] Encontrou algo!
```

Como podemos ver não nenhum novo arquivo foi encontrado com a palavra chave **one_mind**, no entando a imagem **paisagem.jpg** não está nessa pasta **imagens** onde o nosso programa roda.

Então vamos rodar na mão o comando `steghide` para conseguirmos extrair os arquivos escondidos da imagem **paisagem.jpg** usando a senha **one_mind**.

```sh
steghide extract -sf paisagem.jpg
```

```text
OUTPUT

Enter passphrase: 
```

Nesse momento vamos passar a frase **one_mind**.

```text
OUTPUT

wrote extracted data to "tux.jpg".
```

Agora podemos ver que um novo arquivo, chamado **tux.jpg** foi extraído da imagem **paisagem.jpg**.

```sh
file tux.jpg
```

```text
tux.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, baseline, precision 8, 800x800, components 3
```

Aqui podemos ver que realmente se trata de uma imagem com extensão `JPG`.

```sh
head -n5 tux.jpg
```

```text
OUTPUT

����JFIF��C
 










▒▒��C
�  "��

���}!1AQa"q2��#B��R��$3br�

```

O comando `head` não nos disse nada, então vamos tentar ver se o comando `tail` será msia útil.

```sh
tail tux.jpg
```

```text
(����(���(����Q@
                E�P(�����dS���F��x���▒l�U\e
�
 ���F�������_���(~���z��v��z��<S��~���&���>��=�����XK��z�k�=��1x;�J�g����/V�%{S����kԮi
��J��=Ro�zY�D���~L�6�a+�G�=��a%�g<T���}���p~    ig����A�#�������<������S
                                                                        ��|T/�A���?�Q�Y�q�)��V�{\���pl��~�j`�3�e�:(�����M%�*�l��U�?�Ґ�я����%�Y�՜����\▒��}W���.�y�t���I�n?��(���2y[��m+��b���4Y��>z��AE_�_������$zr�\��־�ӿg���$kY'�!d|�z>��>�!��Hb^��W�d�:�\l��E����ppq�y�g�I�+xx�.���h��M$����IRn�i�+�h����i,�[#�� �+�Pu}��xN?�=��
                    ��)�Y�a�A��U��Xx~u�e⇈vʎ��#��>����p�}C밹�#
­G��z�^�g��JO�g�t[���_-/3�?y������                     ���q�(^����(����5f�����X�a<H?�?:����t[��m)�𣴡����m.Ϳ�?y����8��2�~�O������GL���}ӿ�I���������������g▒�+D>��k*�^{�K�N�=+�W����?��Wmi
                                                                                      ����=
                                                                                           ��i�w���S��K�m��ɞ5m޻}��M<�Q�{�J���w�σ�5i`��@�1�ST�+�{D��!�m�1�k���b��C��J��V�.j�Z�d�#��s/�.H+E▒Q����>�A�*@1_��QKE%���%�i(���P�E%QKր���)h����J(��_��P�ZJ(��ZJZJ:�KIE������PKEQҒ�;��Q@-�R�Q@��({�E%QEQK@     E�����RQER�@ZJ(���-%R�@    A\�Z(#�j�=�~���0d��'�����J��@�<�@��B�v��!�]�RyB�9������
                                                                               ���|��G�(���_�����ݮ��Z<�@���������V��c�P�
                     h���ո�B㊼-(Pu�Qڕ������%��x��ij����C▒4϶���N���k���i�H>��d/�h��_��C�
<�翲����
8D�P��S▒Z�{{b��b�.)h���R�QKI@-%-%���4zZ�q�Vג
ZJZJ?���)(��
(�����O�F(����(����RR�
)i(��(�Q@-'ݥ����▒Z)(�����b��
LQK@    KE�QK@  E-�R�PE-�~��PR�Q@
                                 IKE���Q�)h����RъJ)E%w��QKI@(�P`R�QҀh��)i(6Ѷ��Z0(�����i�▒n�6�њ6Ѵ
▒(��(��1E-�QK@  E-!��)h)i)h(������ZO�KIE��
```

Por mais que o comando `tail` tenha trazido mais coisas ele não foi tão útil quanto o esperado, já que não há uma informação clara sendo exibida.


<br />

### img_zen.dd

Como se trata de um arquivo de imagem (por conta da extensão `dd`) devemos primeiramente monta-lo no diretório `analise` que haviamos criado na parte de [Configurações Iniciais](#configurações-iniciais).

```sh
sudo mount -o ro,noexec img_zen.dd analise/
```

Abrindo a pasta **analise** e verificando os arquivos existentes dentro dela, nos deparamos com um volume muito grande.

```sh
cd analise && ls
```

```text
 back.gif           'data (17ª cópia)'  'data (outra cópia)'   gray25.xbm       subplots.gif
 back_large.gif     'data (18ª cópia)'   earth.gif             hand.gif         subplots_large.gif
 data               'data (3ª cópia)'    earthris.gif          hand_large.gif   tcllogo.gif
'data (10ª cópia)'  'data (4ª cópia)'    face.xbm              home.gif         teapot.ppm
'data (11ª cópia)'  'data (5ª cópia)'    filesave.gif          home_large.gif   whatis.1
'data (12ª cópia)'  'data (6ª cópia)'    filesave_large.gif    letters.xbm      whatis.1.gz
'data (13ª cópia)'  'data (7ª cópia)'    flagdown.xbm          move.gif         xml
'data (14ª cópia)'  'data (8ª cópia)'    flagup.xbm            move_large.gif   zoom_to_rect.gif
'data (15ª cópia)'  'data (9ª cópia)'    forward.gif           noletter.xbm     zoom_to_rect_large.gif
'data (16ª cópia)'  'data (cópia)'       forward_large.gif     pattern.xbm
```

Para não perdermos tempo analisando arquivo por arquivo vamos começar buscando os arquivos que foram apagados. Vamos voltar para pasta `Caso_Steg` e rodar o comando abaixo.

```sh
fls -adpr img_zen.dd
```

```text
OUTPUT

-/r * 130-128-2:        .Trash-0/info/we.jpg.trashinfo.ntfs-3g-0000000001
r/- * 0:        data (14ª cópia)
r/- * 0:        xml/zen.jpg
-/r * 125-128-2:        xml/paisagem.jpg
-/r * 121-128-2:        meditation.mp3
-/r * 16:       $OrphanFiles/OrphanFile-16
-/r * 17:       $OrphanFiles/OrphanFile-17
-/r * 18:       $OrphanFiles/OrphanFile-18
-/r * 19:       $OrphanFiles/OrphanFile-19
-/r * 20:       $OrphanFiles/OrphanFile-20
-/r * 21:       $OrphanFiles/OrphanFile-21
-/r * 22:       $OrphanFiles/OrphanFile-22
-/r * 23:       $OrphanFiles/OrphanFile-23
```

Como podemos ver existem vários arquivos que foram excluído dessa imagem, mas os **OrphanFiles** foram destruídos, e normalmente não há dados neles.

De todos os dados retornados existem somente 3 deles que nós chamam a atenção:

- xml/zen.jpg
- xml/paisagem.jpg
- meditation.mp3

No entanto como o inode do arquivo **zen.jpg** encontrado dentro da pasta **xml** está com o valor **zero** não conseguiremos recuperá-lo.

Vamos então criar uma pasta para que possamos lidar mais facilmente com os arquivos que serão recuperados.

```sh
mkdir recovered && cd recovered
```

Para recuperarmos o arquivo deletado podemos utilizar o comando `icat` do próprio Linux, passando o nome da imagem e o inode do arquivo excluído, enviando para um novo arquivo (que terá o mesmo nome).

```sh
icat ../img_zen.dd 125-128-2 > paisagem.jpg
```

```sh
icat ../img_zen.dd 121-128-2 > meditation.mp3
```

Olhando a imagem recuperada podemos ver que ela também tem **11 MB**, que é o mesmo tamanha da imagem [paisagem.jpg](#paisagemjpg) analizada anteriormente.

No entanto o arquivo `meditation.mp3` não roda, o que pode significar que seus dados foram alterados ou até mesmo que esse arquivo não se trata de um **MP3**.

Para verificarmos isso vamos rodar o comando **file**.

```sh
file meditation.mp3
```

```text
OUTPUT

meditation.mp3: ASCII text, with very long lines (2816)
```

Aqui podemos ver que esse arquivo não é de fato um **MP3**, mas sim um **ASCII** em texto com mais de 2800 linhas.

Como se trata de um arquivo muito grande não vamos puxar todo o texto utilizando o comando **strings**, vamos pegar somente o cabeçalho.

```sh
head -n5 meditation.mp3
```

```sh
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
```

Podemos observar que o arquivo traz um padrão bem estranho que se repete a cada linha, sendo que ao final das linhas podemos observar as palavras **mem.lime**. Isso nos indica se ele se trata de um **arquivo de memória**.

Vamos então buscar pelo rodapé deste arquivo para ver se encotnramos algo diferente.

```sh
tail meditation.mp3
```

```text
OUTPUT

EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^
1. esvazi3_@_x1cara
@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
EMiL^A^@^@^@^@^P^@^@^@^@^@^@<FF><FB>    ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@mem.lime
```

Aqui podemos notar que existe uma linha com texto claro, que diz **1. esvazi3@_x1cara**, que é justamente o tipo de mensagem estranha que estamos procurando.

Caso no terminal essa mensagem não esteja tão facilmente visível e você queira extrair todo o texto desse arquivo para um `.txt` podemos rodar o comando abaixo.

```sh
strings -a meditation.mp3 > strings.txt
```

Vamos agora criar umm arquivo chamado `words.txt` para armazenar todas as palavras que encontrarmos.

```sh
touch words.txt
```

<br />

## Palavras encontradas

```text
esvazi3_@_x1cara
xamanism
one_mind
```

<br />

## O que deixamos passar?

Na sessão [X] quando rodamos o comando `fls -adpr img_zen.dd` acabamos deixando alguns arquivos que foram excluídos para fora das nossas análises.

Para conseguirmos analisar esses arquivos vamos rodar o comando `fls` usando uma abordagem diferente.

```sh
fls -apr img_zen.dd | less
```

```text
OUTPUT

r/r 76-128-2:       home.gif
r/r 77-128-2:       home_large.gif
r/r 109-128-2:      data (12ª cópia)
r/r 70-128-2:       filesave.gif
r/r 4-128-1:        $AttrDef
r/r 8-128-2:        $BadClus
r/r 8-128-1:        $BadClus:$Bad
r/r 6-128-1:        $Bitmap
r/r 7-128-1:        $Boot
d/d 11-144-2:       $Extend
d/d 11-144-2:       $Extend/.
d/d 5-144-3:        $Extend/..
r/r 25-144-2:       $Extend/$ObjId:$O
r/r 24-144-3:       $Extend/$Quota:$O
r/r 24-144-2:       $Extend/$Quota:$Q
r/r 26-144-2:       $Extend/$Reparse:$R
r/r 2-128-1:        $LogFile
r/r 0-128-1:        $MFT
r/r 1-128-1:        $MFTMirr
r/r 9-128-2:        $Secure:$SDS
r/r 9-144-3:        $Secure:$SDH
r/r 9-144-4:        $Secure:$SII
r/r 10-128-1:       $UpCase
r/r 10-128-2:       $UpCase:$Info
r/r 3-128-3:        $Volume
d/d 5-144-3:        .
d/d 123-144-2:      .Trash-0
d/d 123-144-2:      .Trash-0/.
d/d 5-144-3:        .Trash-0/..
d/d 129-144-2:      .Trash-0/files
d/d 129-144-2:      .Trash-0/files/.
d/d 123-144-2:      .Trash-0/files/..
r/r 126-128-2:      .Trash-0/files/we.jpg
d/d 124-144-2:      .Trash-0/info
d/d 124-144-2:      .Trash-0/info/.
d/d 123-144-2:      .Trash-0/info/..
r/r 131-128-2:      .Trash-0/info/we.jpg.trashinfo
-/r * 130-128-2:    .Trash-0/info/we.jpg.trashinfo.ntfs-3g-0000000001
d/d 66-144-2:       .Trash-1000
d/d 66-144-2:       .Trash-1000/.
d/d 5-144-3:        .Trash-1000/..
d/d 69-144-2:       .Trash-1000/expunged
d/d 69-144-2:       .Trash-1000/expunged/.
d/d 66-144-2:       .Trash-1000/expunged/..
d/d 68-144-2:       .Trash-1000/files
d/d 68-144-2:       .Trash-1000/files/.
d/d 66-144-2:       .Trash-1000/files/..
d/d 67-144-2:       .Trash-1000/info
d/d 67-144-2:       .Trash-1000/info/.
d/d 66-144-2:       .Trash-1000/info/..
r/- 70:             .Trash-1000/info/trash.trashinfo
r/r 64-128-2:       back.gif
r/r 65-128-2:       back_large.gif
r/r 97-128-2:       data
r/r 107-128-2:      data (10ª cópia)
r/r 108-128-2:      data (11ª cópia)
r/- * 0:            data (14ª cópia)
r/r 112-128-2:      data (15ª cópia)
r/r 113-128-2:      data (16ª cópia)
r/r 114-128-2:      data (17ª cópia)
r/r 115-128-2:      data (18ª cópia)
r/r 100-128-2:      data (3ª cópia)
r/r 101-128-2:      data (4ª cópia)
r/r 102-128-2:      data (5ª cópia)
r/r 103-128-2:      data (6ª cópia)
r/r 104-128-2:      data (7ª cópia)
r/r 105-128-2:      data (8ª cópia)
r/r 106-128-2:      data (9ª cópia)
r/r 98-128-2:       data (cópia)
r/r 99-128-2:       data (outra cópia)
r/r 84-128-2:       earth.gif
r/r 85-128-2:       earthris.gif
r/r 86-128-2:       face.xbm
r/r 71-128-2:       filesave_large.gif
r/r 87-128-2:       flagdown.xbm
r/r 88-128-2:       flagup.xbm
r/r 72-128-2:       forward.gif
r/r 73-128-2:       forward_large.gif
r/r 89-128-2:       gray25.xbm
r/r 74-128-2:       hand.gif
r/r 75-128-2:       hand_large.gif
r/r 90-128-2:       letters.xbm
r/r 78-128-2:       move.gif
r/r 79-128-2:       move_large.gif
r/r 91-128-2:       noletter.xbm
r/r 92-128-2:       pattern.xbm
r/r 80-128-2:       subplots.gif
r/r 81-128-2:       subplots_large.gif
r/r 93-128-2:       tcllogo.gif
r/r 94-128-2:       teapot.ppm
d/d 96-144-2:       whatis.1
d/d 96-144-2:       whatis.1/.
d/d 5-144-3:        whatis.1/..
r/r 95-128-2:       whatis.1.gz
d/d 116-144-2:      xml
d/d 116-144-2:      xml/.
d/d 5-144-3:        xml/..
r/r 117-128-2:      xml/all.jpg
r/r 118-128-2:      xml/baba.jpg
r/r 119-128-2:      xml/bear.jpg
r/r 120-128-2:      xml/budismo.jpg
r/r 122-128-2:      xml/flow.jpg
r/r 127-128-2:      xml/xamnism.jpg
r/r 128-128-2:      xml/zen.jpg
r/- * 0:            xml/zen.jpg
-/r * 125-128-2:    xml/paisagem.jpg
r/r 82-128-2:       zoom_to_rect.gif
r/r 83-128-2:       zoom_to_rect_large.gif
r/r 110-128-2:      data (13ª cópia)
r/r 111-128-2:      data (14ª cópia)
-/r * 121-128-2:    meditation.mp3
V/V 132:            $OrphanFiles
-/r * 16:           $OrphanFiles/OrphanFile-16
-/r * 17:           $OrphanFiles/OrphanFile-17
-/r * 18:           $OrphanFiles/OrphanFile-18
-/r * 19:           $OrphanFiles/OrphanFile-19
-/r * 20:           $OrphanFiles/OrphanFile-20
-/r * 21:           $OrphanFiles/OrphanFile-21
-/r * 22:           $OrphanFiles/OrphanFile-22
-/r * 23:           $OrphanFiles/OrphanFile-23
```

Dentro de todos esses arquivos da para notar que existe um arquivo chamado **we.jpj** que nós não haviamos visto antes. Então vamos extraí-lo.

```sh
icat ../img_zen.dd 126-128-2 > we.jpg
```
