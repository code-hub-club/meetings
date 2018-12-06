#Java Package
Файлуудыг үйл ажиллагааных нь дагуу ангилан директоруудад хийх явдал. Мөн ижил нэртэй класс ашиглах үед нэрний давхардлын асуудал үүсэхээс зайлсхийхэд тусалдаг. 


Хоёр төрлийн package байдаг: хэрэглэгчийн тодорхойлж өгсөн, заяамал (built-in)

Хэрэглэгчийн тодорхойлж өгсөн package үүсгэх
example.java файлыг ex package-д дараах байдлаар бичиж өгнө.
```Java{.line-numbers}
package ex;
public class Example {
	public static void main(String[] args)
{ System.out.println(“Example”); 
}}
```


##Package хэрэглэх
Package-д хадгалагдсан public хандалттай классуудыг ашиглах дараах 2 арга байдаг.
1. Классын нэрийг түүний агуулагдаж буй package-н замтай хамт бүтнээр тодорхойлж өгөх.
```Java{.line-numbers}
ex.Example example = new ex.Example();
```
1. import түлхүүр үг ашиглах
```Java{.line-numbers}
// ex package доторх public хандалттай бүх классыг дуудаж чадна.
import ex.*; 
// Зөвхөн Example классыг дуудаж чадна. (Бүх классыг биш)
import ex.Example; 
```


Package ашиглахын давуу тал
- Дахин ашиглах боломж
- Илүү зохион байгуулалттай
- Нэрний давхардлаас зайлсхийх

Naming a Package
Package-д нэр өгөхдөө бүгдийг нь жижиг үсэгээр бичиж өгдөг. Class болон interface-н нэртэй давхцал үүсгэхгүйн тулд. 

## Package-ийн шатлал
```Java{.line-numbers}
import java.awt.*
import java.awt.color.*
```

## Нэрний давхардал
a болон b адилхан v гэсэн класстай
```Java{.line-numbers}
v w; // Гэж болохгүй
a.v w // Бүтэн замыг нь зааж өгөх ёстой 
```

