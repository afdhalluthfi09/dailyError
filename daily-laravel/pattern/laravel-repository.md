# Repository

repository repsentasi dari cara memishakan bisnis logic dari proses controller dengan models dengan begini proses data dibalik antara meraka tidak terlihat dan memudahkan bagi para develop melalakukan moc data dan testing data;

###### pattern repository.

ini opsional tapi ini  langkah paling mendekati jika terjadi pengengbangan besar berikutnya jika di butuhkan, dengan membuat parent class dari tiap child class  repo yang di buat beradasarkna nama case seperti feature,model atau masih banyak lagi, kasus ini case child class berdasarakn models yang di butuhkan.

* Buat Parent Class repsository dengan nama dasar `Respository.php`.

  ```php
  use Illuminate\Database\Eloquent\Model;
  class BaseRepository
  {
      protected $model;

      public function __construct(Model $model)
      {
          $this->model = $model;
      }

      public function create(array $data)
      {
          return $this->model->create($data);
      }

      public function find($id)
      {
          return $this->model->find($id);
      }

      public function update($id, array $data)
      {
          $record = $this->find($id);

          if ($record) {
              $record->update($data);
              return $record;
          }

          return null;
      }

      public function delete($id)
      {
          $record = $this->find($id);

          if ($record) {
              $record->delete();
              return true;
          }

          return false;
      }
  }

  ```
* Buat Chilld Class Respository lalu extend dengan Parentnya.

  ```php
  use App\Models\Kelas;
  class KelasRepository extends BaseRepository
  {
      public function __construct(Kelas $kelas)
      {
          parent::__construct($kelas);
      }

      // Method-method khusus untuk model Kelas
      public function deletById($id)
      {
           return $this->delete($id);
       }
  }
  ```
