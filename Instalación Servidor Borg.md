Instalación Servidor Borg



**Actualizamos los repositorios**
```
apt-get update
```

Y a continuación actualizamos la paqueteria del servidor

```
apt-get upgrade -y
```

**Establecer el idioma**

```
dpkg-reconfigure locales
```

Elegimos nuestro idoma

![e72afcc63aff2265837ff467eced4529.png](../../../_resources/b664c5bece8f4704be1f360388811e52.png)

Y lo seleccionamos para establecerlo por defecto

![e035f7e8eb906836d7c2034c898b63e0.png](../../../_resources/e3672efb530544b7a1106468eee391ff.png)

```
locale -a | grep "UTF-8"
export LC_ALL=$(locale -a | grep UTF-8)
```

### Instalamos borgbackup

```
apt install borgbackup
```

En mi caso yo he montado un punto de montaje en **/var/backups** de un disco de 500G para almacenar las copias

```
df -hP /var/backups
```
>Filesystem                               Size  Used Avail Use% Mounted on
>
>/dev/mapper/vg_backups-vm--100--disk--0  492G   73M  467G   1% /var/backups

### Creamos un usuario de gestión
```
adduser ubkag
```

### Iniciamos el repositorio

```
borg init --encryption=repokey /var/backups/repo
```

Nos pedira la contraseña que queramos establecer al repositorio

>borg init --encryption=repokey /var/backups/repo
>
>Enter new passphrase:

Creamos un primer backup de prueba

```
borg create /var/backups/repo::backup_ubkag_inicial /home/ubkag
```

### Ver lista de copias almacenadas

```
borg list /var/backups/repo
```

>borg list /var/backups/repo
>
>Enter passphrase for key /var/backups/repo: 
>
>backup_ubkag_inicial                  Fri, 2021-05-21 20:12:29 [1f8c710dbad3f4979230f8910f657f9addcbbb81576ea61600d59460e72c3677]

### Eliminar copias antiguas

```
borg prune -v --list --keep-daily=10 --keep-weekly=6 --keep-monthly=3 ::
```

