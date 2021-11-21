**custom-boot-nte**

Parâmetros de inicialização customizados, adequado para sistemas live, até mesmo a partir de sistemas já instalados. Imaginado para funcionar de forma semelhante ao *casper*, mas sem aplicar alterações na configuração do sistema base, como a adição de um superusuário sem senha ou alterações do polkit.

Com esse projeto, qualquer sistema pode ser usado como um sistema live, até mesmo um sistema já instalado.

Reconheço que testei apenas com o Ubuntu esse script de inicialização. Foi testado com Ubuntu 18.04, 20.04 e 22.04 (development branch) 😅️

Para usá-lo na inicialização, o parâmetro `boot=custom-boot-nte` precisa ser adicionado, e o módulo do kernel `overlay` deve estar habilitado. Caso haja dúvidas se a configuração foi aplicada, adicione também `nte=1` à linha de comando do Linux. Se tudo der certo, a seguinte arte deve aparecer:

```
#######################################################################
#######################################################################
#######################################################################
##*    `##########     $##%                    %##:                  ,#
##*     `%########,    $##%                    %##:                  ,#
##*       *#######,    $##%,,,,,,,,    ,,,,,,,,%##:    ,,,,,,,,,,,,,,+#
##*        :######,    $##########-    -##########:    ,###############
##*         ,$####,    $##########-    -##########:    ,###############
##*    :-    .$###,    $##########-    -##########:                 :##
##*    :#:     %##,    $##########-    -##########:                 :##
##*    :##*     +#,    $##########-    -##########:     ,,,,,,,,,,,,:##
##*    :###%`    -,    $##########-    -##########:    ,###############
##*    :####$,         $##########-    -##########:    ,###############
##*    :######:        $##########-    -##########:    ,###############
##*    :#######+       $##########-    -##########:     ``````````````$
##*    :########%`     $##########-    -##########:                   $
##*    :#########$     $##########-    -##########:                   $
#######################################################################
#######################################################################
#######################################################################
```
Como isso foi pensado para expandir em relação ao *casper*, o custom-boot-nte também suporta arquivos *squashfs*. A seguir estão dois exemplo válidos do parâmetro root da inicialização:

A partir de uma partição:
```
boot=custom-boot-nte root=/dev/sda2 rootfstype=ext4
```
A partir de um arquivo:
```
boot=custom-boot-nte root=UUID=ABCD-1234/oSistema.squashfs rootfstype=squashfs
```
Note que o UUID `ABCD-1234` seria, por exemplo, o UUID de uma partição FAT32 que contém o arquivo `oSistema.squashfs`.

Ao ligar o sistema, o `root` aparecerá chamado `/mnt/ro-fs`, mas existem duas possibilidades, dependendo se for uma partição ou um arquivo:
- Se for uma partição, o ponto de montagem da raiz será `/mnt/ro-fs`;
- Se for um arquivo, o arquivo estará em `/mnt/ro-fs` e a partição onde o arquivo está contido estará montada em `/mnt/contem-arq-root`

Notas:

- Como esse será um sistema live usando overlay, ele acabará usando memória RAM para o armazenamento temporário. Esse armazenamento foi especificado para ser 80% da memória RAM, valor que me pareceu satisfatório: ele foi testado em sistemas usando desktop Xfce 4.12, 4.14 e 4.16 que possuem entre 1 e 20 GB de memória RAM.
- Se o `/etc/fstab` lista partições leitura-escrita além da raiz (`/home`, por exemplo), **essas partições serão montadas como especificadas (talvez LEITURA-ESCRITA!!!)**;
- Não cheguei a testar, mas se tem mais locais além do `/etc/fstab` que definem a raiz como leitura-escrita (nunca se sabe...), pode ser que o modo somente leitura não funcione.

Baseado no arquivo `/usr/share/initramfs-tools/scripts/local` disponível no Ubuntu 20.04. Também baseado no `casper`.
