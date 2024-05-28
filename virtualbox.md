### backup:
   + сжать большие файлы
   + сохранить файлы где хочешь
   + на школьном маке: создать папку с таким же названием и по тому же пути в goinfre, скачать туда файлы, распаковать, запустить virtualbox (не менять конфигурацию в virtualbox)
   + на другом компе: скачать и разархивировать конфигурацию, virtualbox - Инструменты - зелёный плюсик "Добавить", указав папку с файлом Debian.vbox и прочими файлами

### Error
```
"Could not open the medium '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi'.
VDI: error reading pre-header in '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi' (VERR_IS_A_DIRECTORY).
VD: error VERR_VD_VDI_INVALID_HEADER opening image file '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi' (VERR_VD_VDI_INVALID_HEADER).
Result Code: 
NS_ERROR_FAILURE (0x80004005)
Component: 
MediumWrap
Interface: 
IMedium {ad47ad09-787b-44ab-b343-a082a3f2dfb1}" 
```
* Install HxD Hex Editor to open .vdi file as streams of bytes (http://download.cnet.com/HxD-Hex-Editor ... tag=button)
* pre-header = the first 72 bytes (http://forums.virtualbox.org/viewtopic.php?t=52)
* create a new vm `test.vdi` with all default options
* Open `test.vdi` and copy its first 72 bytes
* overwrite them in the invalid vdi file that you want to fix
* reboot your vm using the modified vdi
