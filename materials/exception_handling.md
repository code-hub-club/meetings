# Exception Handling
`JVM` нь програм ажиллах явцад Java програмчлал хэлний хувьд утга зүйн алдаа гаргасан үед алдаатай байгааг програмд тайлбар (`exception`) байдалаар илгээдэг.
Энэ гарсан алдааг Java хэлэнд програмистын тодорхойлж өгсөн газар руу нь програмын удирдлагыг шилжүүлнэ.

Java хэлний хувьд `Throwable`-н яг доод тал дахь `subclass`-ууд нь `Exception` болон `Error` байна. Тэд нар нь бас доор доороо өөрийн гэсэн `subclass`-уудтай байна. Үүнийг доорх зурагаас харна уу.
![Java Exception Hierarchy](exception_java.png)

Бүх гарч болохуйц `exception` нь `Throwable` болон үүний `subclass`-уудаас байна. Энэ объект нь тухайн `exception` үүссэнээс хойш түүнийг шийдэх `handler` таарах хүртлэх бүх мэдээлэлийг хадагалан.

Ямар нэгэн `exception` шидэх үед `JVM` нь нэг нэгээр нь хийх үйлдлээ эхлүүлдэг, харин ажиллаж байгаа үйлдлээ (any expressions, statements, method and constructor invocations, initializers, and field initialization expressions) гүйцэт дуусгах албагүй. Энэ процесс нь түүнийг шийдэх `handler` ,тухайн  `exception`-ийн классын нэр эсвэл түүний `superclass`-н нэр олдох хүртэл үргэлжилнэ. Хэрэв ямар нэгэн `handler` байхгүй бол автоматаар `uncaught exception handler`-т удирлагаа шилжүүлнэ.

## `Exception`-ийн төрөл

`Exception` болон `Error` нь `Throwable`-ийн яг доод талын `subclass` юм.
- `Exception` нь сэргээгдэж болох бүх алдаануудын `superclass` юм.
- `Error` нь сэргээгдэх боломжгүй бүх алдаануудын `superclass` юм.
- `unchecked exception` нь бүх `RuntimeException` болон `Error` классууд юм.
    - Компайл хийгдэж байгаа үед шалгахгүй, харин програм ажиллаж буй үед гарч ирдэг `exception` юм.
    Жишээ нь: NullPointerException
    ```Java{.line-numbers}
    Object obj = null;
    obj.toString();  // This statement will throw a NullPointerException
    ```
- `checked exception` нь үлдсэн бүх класс юм.
    - Компайл хийгдэж байгаа үед шалгадаг ба түүнийг `throw` эсвэл `try ... catch` хйих ёстой.
    Жишээ нь: java.io.IOException
    ```Java{.line-numbers}
    public void ioOperation(boolean isResourceAvailable) {
    if (!isResourceAvailable) {
            throw new IOException();
        }
    }
    ```
    Дээрх код компайл хийгдэхгүй, яагаад гэвэл энэ кодонд алдаа гарч болох боловч түүнийгээ шийдээгүй. Үүнийг хоёр янзаар засч болно.
    1. `try catch` ашиглах
    ```Java{.line-numbers}
    public void ioOperation(boolean isResourceAvailable) {
        try {
            if (!isResourceAvailable) {
                throw new IOException();
            }
        } catch(IOException e) {
            // Handle caught exceptions.
        }
    }
    ```
    1. `throw` ашиглах
    ```Java{.line-numbers}
    public void ioOperation(boolean isResourceAvailable) throws IOException {
        if (!isResourceAvailable) {
            throw new IOException();
        }
    }
    ```

## `Exception`-ны үүсэх шалтгаан
- `Throw` statement ажиллахад
- `JVM`-д шалгагдсан доголдолтой явц
    - Java хэлний утга зүйн хувьд алдаатай, тоог тэгт хуваах
    - `LinkageError` үүссэн үед
    - Дотоод нөөц дууссан үлмаас алдаа заах
  
[Дэлэгрэнгүй](https://docs.oracle.com/javase/specs/jls/se7/html/jls-11.html)