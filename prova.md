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

## Tools

```text
Will be avaliable
```

<br />

## Objectives

- Recovered files
- Found passwords
- Final result

<br />

## Getting started

To extract all information with the **mount** command, first we need to create a folder called **analise**.

After the **analise** folder was created, run the **mount** command to extract all information of the **img_p1.dd** file.

```sh
mkdir analise
```

```sh
sudo mount -o ro,noexec,offset=$((512*2048)) img_p1.dd analise
```

<br />

Let`s see all the files excluded on this **dd** extension running the command below.

```sh
fls -adpro 2048 img_p1.dd
```

```sh
icat -o 2048 img_p1.dd > NAO_SEI_O_RESTANTE_DO_COMANDO
```
