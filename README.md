# Понимание JVM

- Старт JVM.
- Выделение памяти (Stack, Heap, Metaspace).
- Bootstrap Class Loader загружает базовые классы 
- Extension Class Loader для загрузкой расширений/дополнений.
- в Platform Class Loader передается путь класса
- Application Class Loader загружает пользовательские классы, байткод класса JvmComprehension, проверяет код, доступность классов, полей, методов, ссылок на системные классы
- данные класса загружаются в MetaSpace

## Код с комментариями
```
public static void main(String[] args) { //выделение памяти в стеке для main
        
        int i = 1;  // 1 Переменная "i" типа int со значением 1 
                    //помещается в Stack Memory -> фрейм main
        
        Object o = new Object();        // 2 Создан объект типа Object, 
                                        // помещен в heap (кучу), 
                                        // в Stack Memory -> фрейм main
                                        // размещается переменная "o" - ссылка на объект   
        
        Integer ii = 2;                 // 3 Создан объект типа Integer, 
                                        //помещен в heap (кучу), 
                                        // в Stack Memory -> фрейм main размещается 
                                        // переменная ii со ссылкой на объект    
		
        printAll(o, i, ii);             // 4 в Stack Memory создается фрейм printAll, 
                                        //в нем - переменная "o" со ссылкой на объект Object,
                                        //переменная i со значением 1, переменная ii со 
                                        //ссылкой на объект Integer (Integerstack)

        System.out.println("finished"); // 7 в Stack Memory создается фрейм println, 
                                        //в heap (куче) - объект типа String, 
                                        //во фрейм println ссылка на объект 
                                        //String со значением "finished"
    } 
    //из Stack Memory удаляется фрейм main(), 
    //чистятся ссылки и значения 
    //String[] args, int i, Object o, Integer ii, 
    //объекты в куче доступны для сборщика мусора
	
    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5 в heap (куче) создан объект 
                                                    //типа Integer со значением 700, 
                                                    //в Stack Memory -> фрейм printAll 
                                                    //создана переменная uselessVar со 
                                                    //ссылкой на этот объект в куче. 
                                                    //Объект не используется, 
                                                    //компилятор может его выбросить

        System.out.println(o.toString() + i + ii);  // 6  в Stack Memory создан 
                                                    //фрейм o.toString, 
                                                    //в heap (куче) - объект типа String, 
                                                    //из Stack Memory удаляется 
                                                    //фрейм o.toString. 
                                                    //В Stack Memory создан 
                                                    //фрейм System.out.println, 
                                                    //во фрейме - ссылка на объект типа 
                                                    //String, в heap (куче) с результатом 
                                                    //выполнения метода o.toString(), значения
                                                    //переменных i и ii (ссылкой на объект 
                                                    //Integer (Integerstack)).
                                                    //Из Stack Memory удаляется фрейм 
                                                    //System.out.println
													
    } 
    //из Stack Memory удаляется фрейм printAll, 
    //ссылки и значения переменных 
    //Object o, int i, Integer ii, Integer uselessVar, 
    //связанные с фреймом объекты в куче доступны для сборщика мусора
}
```
