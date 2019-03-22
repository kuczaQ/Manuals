`$ gdisk /dev/sda`  
  
ta tu typowo, dla uproszczenia po pipe (|) nazwy urzadzen w tym scenariuszu

    > /boot - ok. 300M      | /dev/sda1
    > swap  - ok. 6G        | /dev/sda2
    > /     - ok. 32G       | /dev/sda3
    > /home - to co zostalo | /dev/sda4

lecim z formatami  


`$ mkfs.vfat /dev/sda1 -n EFI -F 32`  # make vfat file system  
`$ mkswap /dev/sda2`  # make swap  
`$ mkfs.ext4 /dev/sda3 -L ROOT` # make ext4 file system  
`$ mkfs.ext4 /dev/sda4`  

teraz montujemy

`$ mount /dev/sda3 /mnt`  
`$ cd /mnt`  
`$ mkdir home`  # make directory  
`$ mkdir boot`  
`$ mount /dev/sda1 boot`  
`$ mount /dev/sda4 home`  

wlaczamy swapa

`$ swapon /dev/sda2`  

`$ wifi-menu`  

ta to co teraz bedzie to na jednej linijce wszystko, a rzeczy po # to komentarze

```
$ pacstrap /mnt base # arch
                base-devel # gcc itp. itd.
                vim # no normalnie vim
                networkmanager # duh
                bash-completion # jak wcisniesz tab cos piszac to ci moze podpowie
                tmux # znaczy terminal multiplexer, dobre gowno, ogarnij
                # ogolnie mozesz se wpisac co chcesz tutaj i sie zainstaluje
```
lecim z fstab, czyli file system table
opcja -L mowi, ze ma wygenerowac to na podstawie etykiet
> patrz linjka 11 tego pliku

`$ genfstab /mnt -L > /mnt/etc/fstab`  

wchodzimy to instalacji linuxa co sie pobrala wlasnie

`$ arch-chroot /mnt`  # arch change root

instalujemy bootloadera

`$ bootctl install`

i nakurwiamy konfiguracje bootloadera

`$ cd /boot/loader`
```
$ vim loader.conf

    > default arch
    > timeout 3
    > editor 0
```
`$ cd entries`
```
$ vim arch.conf
   
    > title   Arch Linux          
    > linux   /vmlinuz-linux      
    > initrd  /initramfs-linux.img
    > options root=LABEL=ROOT rw  
```
i w sumie to tyle, teraz sie wlaczyc powinien
ale mozna jeszcze wlaczyc networkmanager przy starcie i ustawic haslo roota


`$ systemctl enable NetworkManager`  
`$ passwd`  


`$ exit`  
`$ reboot`  
