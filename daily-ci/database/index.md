# Koneksi Database CI V3

###### memanggilnya di setiap method pada contorller

asumsinya hanya satu class controller saja yang membuthukan method terhubung dengan koneksi database.

```php
<?php
class nameController extends CI_Contorller
{
	public function getData()
	{
	    $this->load->database();
	}
}
```

###### memanggil di menggunakan constructor pada controller.

asumsinya hanya beberpaa controller dengan semua method yang didalamnya terhubung dengan koneksi database.

```php
<?php
class nameController extends CI_Controller
{
	public function __constructor()
	{
		parent::__constructur();
		$this->load->database();
	}
}
```

###### memanggilnya dengan autoload.

asusminya semua class controller yang di inisialisasi terhubung dengan konkesi database.

* pergi ke path `config/autoload.php` lalu cari `$autoload['libraries']`

  ```php
  <?php
  /*
  | -------------------------------------------------------------------
  |  Auto-load Libraries
  | -------------------------------------------------------------------
  | These are the classes located in system/libraries/ or your
  | application/libraries/ directory, with the addition of the
  | 'database' library, which is somewhat of a special case.
  |
  | Prototype:
  |
  |	$autoload['libraries'] = array('database', 'email', 'session');
  |
  | You can also supply an alternative library name to be assigned
  | in the controller:
  |
  |	$autoload['libraries'] = array('user_agent' => 'ua');
  */
  $autoload['libraries'] = array('database');
  ```
* dan tidak perlu lagi memanggilnya $this->load->database() di setiap constructor di class controller yang dibuat.
