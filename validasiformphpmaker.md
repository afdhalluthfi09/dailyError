# PHPMAKER VALIDASI FORM 
di phpmaker untuk melakukan validasi form bisa menggunakan seperti ini
```PHP
<form name="flokasiadd" id="flokasiadd" class="<?php echo $lokasi_add->FormClassName ?>" action="<?php echo CurrentPageName() ?>" method="post">
//untuk pengecekan form
<?php if ($Page->CheckToken) { ?>
    <input type="hidden" name="<?php echo Config("TOKEN_NAME") ?>" value="<?php echo $Page->Token ?>">
<?php } ?>
</form>