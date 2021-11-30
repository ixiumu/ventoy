# Ventoy

The [Ventoy Poly Dark](https://github.com/jfcherng/Ventoy-theme-poly-dark) Ventoy theme modified.

## EFI partition Install

ventoy_wimboot.img & ventoy_vhdboot.img & ventoy.json -> (VTOYEFI)\ventoy\

themes\ ->  (VTOYEFI)\grub\themes\

grub.cfg -> (VTOYEFI)\grub\expand.cfg

BOOT\GRUB\GRUB -> (VTOYEFI)\tool\grub4dos

### (VTOYEFI)\grub\grub.cfg

```
-
#Load Plugin
if [ -f $vtoy_iso_part/ventoy/ventoy.json ]; then
    clear
    vt_load_plugin $vtoy_iso_part
    clear
else
    vt_check_json_path_case $vtoy_iso_part
fi
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
else
    vt_check_json_path_case $vtoy_iso_part
fi
-
function ventoy_ext_menu {
    if [ -e $vt_plugin_path/ventoy/ventoy_grub.cfg ]; then
        set ventoy_new_context=1
        configfile $vt_plugin_path/ventoy/ventoy_grub.cfg
        unset ventoy_new_context
    else
       echo "ventoy_grub.cfg NOT exist."
       echo -e "\npress ENTER to exit ..."
       read vtInputKey
    fi
}
+
function ventoy_ext_menu {
    if [ -e $vtoy_iso_part/grub.cfg ]; then
        set ventoy_new_context=1
        configfile $vtoy_iso_part/grub.cfg
        unset ventoy_new_context
    elif [ -e $vtoy_efi_part/grub/expand.cfg ]; then
        set ventoy_new_context=1
        configfile $vtoy_efi_part/grub/expand.cfg
        unset ventoy_new_context
    else
       echo "expand.cfg or grub.cfg NOT exist."
       echo -e "\npress ENTER to exit ..."
       read vtInputKey
    fi
}
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

## Hide EFI partition

Use bootice to set partition hiding [EF] -> [16]