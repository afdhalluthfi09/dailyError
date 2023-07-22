# PDO CLASS 
ini template buatan kalo ada yang minta koneksinya pake pdo 🙂

```php
class DatabaseWarper {
    private $host;
    private $user;
    private $port;
    private $db_name;
    private $db_pass;
    private $dbh;
    private $stmt;

    public function __construct()
    {
        $dsn = 'msyql:'.$this->host.';dbname='.$this->db_name;

        // koneksi
        $options=[
                PDO::ATTR_PERSISTENT=>true,
                PDO::ATTR_ERRMODE =>PDO::ERRMODE_EXCEPTION
        ];
        try {
            
            $this->dbh=new pdo($dsn,$this->user,$this->db_pass, $options);
        } catch (PDOException $e) {
            //throw $th;
            die($e->getMessage());
        }
    }

    public function query($query){
        $this->stmt = $this->dbh->prepare($query);
    }

    public function bind ($param,$value,$type=null){
        if(is_null($type)){
            switch (true) {
                case is_int($value):
                    $type = PDO::PARAM_INT;
                    break;
                case is_bool($valeu):
                    $type = PDO::PARAM_BOOL;
                    break;
                case is_null($valeu):
                    $type=PDO::PARAM_NULL;
                    break;        
                default:
                    $type=PDO::PARAM_STR;
                    break;
            }
        }
        $this->stmt->bindValue($param,$value,$type);
    }

    public function execute(){
        $this->stmt->execute();
    }

    public function ambilSemua(){
        $this->execute();
        $this->stmt->fetchAll(PDO::FETCH_ASSOC);
    }

    public function ambilPerRow(){
        $this->execute();
        $this->stmt->fetch(PDO::FETCH_ASSOC);
    }
}
```