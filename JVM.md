
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}

Система подгрузки классов загружает класс JvmComprehension в Metaspace.
В стэке создаётся фрейм main().

int i = 1;                      // 1

Примитивная переменная i сохраняется в стэке в созданном фрейме main().

Object o = new Object();        // 2

Объект о создаётся в куче, ссылка на него хранится в стэке.

Integer ii = 2;                 // 3

Объект создаётся в куче, ссылка на него хранится в стэке.

printAll(o, i, ii);             // 4

Происходит вызов метода printAll, из-за чего создаётся следующий фрейм printAll(). Так как Object и Integer - это ссылочные типы данных, в фрейме создаются свои собственные ссылки на объекты. Сохраняется и своя собственная примитивная переменная i. 

Integer uselessVar = 700;                   // 5

В куче создаётся объект типа Integer, в стэке сохраняется ссылка uselessVar. 

System.out.println(o.toString() + i + ii);  // 6

При вызове метода println() в стэке будет создан новый фрейм, куда сложатся переменные: создадутся ссылки на те же объекты и примитивная переменная i. 
Во время работы метода println() в стэке будет создан новый фрейм метода .toString(), после завершения которого его метод уничтожится. 
После выполнения метода println() фрейм printAll() уничтожится. Так как ссылки на объекты Object и Integer, а также PrintStream больше не активны, эти объекты попадают в поле досягаемости сборщика мусора и будут удалены при следующей его сессии.

System.out.println("finished"); // 7

Создастся новый фрейм println(), после срабатывания метода он уничтожится, после чего уничтожится фрейм main(). Объект PrintStream станет досягаем для GarbageCollector'а.