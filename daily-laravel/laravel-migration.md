# Migration

###### cara merubah type data attributte column table

`php artisan make:migration change_type_column_table_name --table=nametable`

```php
public function up()
{
    Schema::table('tablename', function (Blueprint $table) {
        // Batalkan perubahan tipe atribut kolom "name"
        $table->string('name')->change();
    });
}
public function down()
{
    Schema::table('tablename', function (Blueprint $table) {
        // Kembalikan tipe atribut kolom "name" ke tipe integer
        $table->integer('name')->change();
    });
}
```
