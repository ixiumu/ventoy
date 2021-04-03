# Ventoy

The [Ventoy Poly Dark](https://github.com/jfcherng/Ventoy-theme-poly-dark) Ventoy theme modified.

# EFI Install

ventoy_wimboot.img & ventoy_vhdboot.img -> (VTOYEFI)\ventoy

themes\ ->  (VTOYEFI)\grub\themes\ventoy

ventoy.json -> (VTOYEFI)\ventoy

## (VTOYEFI)\grub\grub.cfg

```
+
#Load Plugin
if [ -f $vtoy_iso_part/ventoy/ventoy.json ]; then
   clear
   vt_load_plugin $vtoy_iso_part
   clear
elif [ -f $vtoy_efi_part/ventoy/ventoy.json ]; then
   clear
   vt_load_plugin $vtoy_efi_part
   clear
fi

-
for vtTFile in ventoy.json ventoy_grub.cfg; do
    if [ -f $vtoy_efi_part/ventoy/$vtTFile ]; then
        clear
        echo -e "\n You need to put $vtTFile in the 1st partition which hold the ISO files.\n"
        echo -e " $vtTFile 放错分区了，请放到镜像分区里的 ventoy 目录下（此目录需要手动创建）！\n"
        echo -e "\n press ENTER to continue (请按 回车 键继续) ..."
        read vtInputKey
    fi
done
```