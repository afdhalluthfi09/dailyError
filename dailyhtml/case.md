# HTML

```html
<div class="form-group row">
                                    <div class="col-sm-12">
                                        <label for="confirm-2" class="block">No Telp / Fax *</label>
                                    </div>
                                    <div class="col-sm-12">
                                        <input name="no_telp_customer" id="no_telp_customer" type="text" class="form-control fax required" data-mask="(999) 999-99999">
                                    </div>
                                    <script>
                                        $(function(){
                                            $('.fax').inputmask({ mask: "(999) 999-99999" });
                                        })
                                    </script>
                                </div>
```

```html
<div class="form-group row">
                                    <div class="col-sm-12">
                                        <label for="no_telp-pic" class="block">No Telp PIC Order *</label>
                                    </div>
                                    <div class="col-sm-12">
                                        <input name="no_telp_picorder" id="no_telp_picorder" type="text" class="form-control telp_pic required" data-mask="9999-9999-9999">
                                    </div>
                                    <script>
                                        $(function(){
                                            $('.telp_pic').inputmask({ mask: "9999-9999-9999" });
                                        })
                                    </script>
                                </div>
```
