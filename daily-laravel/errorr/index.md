# Handle Error
## TryCatch
handle error bersarang pada satu classs:
kita ingin menangkap error dari method lain,dari method utama, disnin case saya beri nama nameMethod1 dan nameMethod2, dimana nameMethod2 akses nameMethod1 
```php
  class SomeClass{

    public function nameMethod1(Request $request){
        try{
            ...code
        }catch(\Exception $ex){
            $ex->originalMethod = __FUNCTION__;
            trow $ex;
        }
    }

    public function nameMethod2(Request $request){
        try{
            ...code
            static::nameMethod1($request);
        }catch(\Exception $ex){
            $callingMethod = isset($ex->originalMethod) ? $ex->originalMethod : 'Unknown';
                if ($callingMethod === 'Unknown') {
                    $backtrace = debug_backtrace();
                    foreach ($backtrace as $frame) {
                        if (isset($frame['class']) && isset($frame['function']) && $frame['class'] === static::class) {
                            $callingMethod = $frame['function'];
                            break;
                        }
                    }
                }
                $templateMessage = "Error : " . $ex->getMessage() . "\nLine : " . $ex->getLine() . "\nFile : " . $ex->getFile() . "\n pada method " . $callingMethod;
                $cc =json_encode($templateMessage);
                return response()->json(["message"=>$cc],400);
        }
    }
  }