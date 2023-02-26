# rEFInd bootloader Install Guide

*Небольшой Guide для удобства.*

Простая команда установки
```
refind-install
```
Это действие найдет ваш раздел EFI и выполнит установку загрузчика.

В некоторых случаях, возникает потребность в установке загрузчика по умолчанию:
```
refind-install --usedefault /dev/sdXY
```
/dev/sdXY - EFI раздел

Список дисков можно посмотреть воспользовавшись утилитой *GParted* либо командой
```
sudo fdisk -l
```

## Создание загрузочных записей
Все загрузочные записи хранятся в файле */EFI/refind/refind.conf*

**Linux Mint** \
Смонтируем ESP предварительно отмонтировав все точки монтирования */boot*, если они имеются.

```
sudo su
umount /boot
mkdir /mnt/esp
mount /dev/sdc1 /mnt/esp
```
Создаем новый каталог **mint** в */mnt/esp/EFI/*

```
sudo mkdir /mnt/esp/EFI/mint
```

Теперь, копируйте ваше ядро и initramfs в ESP из */boot* системы
в каталог */mnt/esp/EFI/mint*

```
cp /boot/vmlinuz-linux /mnt/esp/EFI/mint
cp /boot/initramfs-linux.img /mnt/esp/EFI/mint
```

> Ядро и файл initramfs в разных дистрибутивах Linux
> могут именоваться по-разному.

Настройка файла конфигурации

```
...

menuentry "Linux Mint" {
	icon     /EFI/refind/icons/os_mint.png
	volume   "Linux Mint"
	loader   /EFI/mint/vmlinuz-linux
	initrd   /EFI/mint/initramfs-linux.img
	options  "root=UUID=your_UUID rw"
  
...
```

Чтобы узнать *UUID* диска нужно выполнить команду

```
sudo blkid
```

Детальнее про rEFInd <br/>
:point_right: https://wiki.archlinux.org/title/REFInd