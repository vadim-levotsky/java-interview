[Вопросы для собеседования](README.md)

# Реактивное программирование
* [Что такое реактивное программирование и чем оно отличается от процедурного программирования?](#что-такое-реактивное-программирование-и-чем-оно-отличается-от-процедурного-программирования)
* [Объясните концепцию потоков данных в реактивном программировании](#объясните-концепцию-потоков-данных-в-реактивном-программировании)
* [Что такое паттерн Observer и как он лежит в основе реактивного программирования?](#что-такое-паттерн-observer-и-как-он-лежит-в-основе-реактивного-программирования)
* [Опишите роль Observable и Observer в реактивном программировании](#опишите-роль-observable-и-observer-в-реактивном-программировании)
* [Что такое backpressure в контексте реактивного программирования?](#что-такое-backpressure-в-контексте-реактивного-программирования)
* [Объясните разницу между Hot и Cold Observable](#объясните-разницу-между-hot-и-cold-observable)
* [Какова роль Подписки в реактивном программировании?](#какова-роль-подписки-в-реактивном-программировании)
* [Как отписаться от потока для предотвращения утечки памяти?](#как-отписаться-от-потока-для-предотвращения-утечки-памяти)
* [Какие есть операторы в Project Reactor и для чего они используются?](#какие-есть-операторы-в-project-reactor-и-для-чего-они-используются)

## Что такое реактивное программирование и чем оно отличается от процедурного программирования?

### Основные принципы

**Процедурное программирование** - это подход, при котором программа состоит из последовательности инструкций, выполняемых одна за другой.
В процедурном программировании акцент делается на определении функций и процедур, которые выполняют определенные задачи.
Этот подход хорошо подходит для решения задач, требующих четкого алгоритма действий.

**Реактивное программирование**, с другой стороны, фокусируется на обработке потоков данных и событий.
В реактивном программировании программа реагирует на изменения в данных или событиях, происходящих в реальном времени.
Реактивное программирование позволяет создавать более гибкие и эффективные системы, способные адаптироваться к изменениям 
в данных и событиях без необходимости явного управления асинхронными задачами.

Процедурное программирование представляет данные в виде единственного значения, хранящегося в переменной.
Реактивное программирование представляет собой непрерывный поток данных, на который могут подписаться несколько наблюдателей.

[к оглавлению](#реактивное-программирование)

## Объясните концепцию потоков данных в реактивном программировании

Концепция потоков данных в реактивном программировании заключается в представлении данных как непрерывно изменяющегося потока, 
который автоматически распространяется через систему. Это отличается от традиционного императивного программирования, где данные 
обычно обрабатываются как отдельные элементы и изменения должны явно инициироваться кодом.

В реактивном программировании данные представлены как поток событий, каждое из которых может содержать новое значение или изменение состояния. 
Система автоматически реагирует на эти изменения, обновляя свое состояние соответствующим образом. Это позволяет создавать 
приложения, которые эффективно обрабатывают большие объемы данных и быстро реагируют на изменения, происходящие в реальном времени.

Примером может служить система управления данными, которая автоматически обновляет пользовательский интерфейс при изменении данных в базе. 
В таком случае, пользовательский интерфейс будет автоматически обновляться каждый раз, когда происходит изменение данных, 
без необходимости явного запроса на обновление со стороны пользователя.

Реактивное программирование основано на шаблоне Наблюдатель (Observer)

### Ключевые компоненты
* **Наблюдаемый (Observable)** представляет источник данных. При изменении его состояния (или при создании новых данных) изменения передаются наблюдателям
* **Наблюдатель (Observer)** подписывается на Observable и получает уведомления о любых изменениях состояния или новых данных
* **Подписка (Subscription)** устанавливает взаимосвязь между наблюдаемым и наблюдателем. Подписка может быть "один к одному" или "один ко многим"
* **Операторы (Operators)** часто называемые функциями преобразования, они позволяют изменять или адаптировать данные из наблюдаемого объекта до того, как они попадут к наблюдателю
* **Планировщики (Schedulers)** помогают управлять временем и порядком выполнения операций в таких сценариях, как фоновая работа и обновления пользовательского интерфейса
* **Subjects** совмещают роли объекта наблюдения и наблюдателя. Это могут быть как источники данных, так и потребители данных

### Процесс передачи данных
* **Эмиссия (Emission)** - данные создаются в наблюдаемом объекте и отправляются его наблюдателям
* **Фильтрация (Filtering)** - операторы могут просматривать входящие данные, пересылая только те, которые соответствуют определенным критериям
* **Трансформация (Transformation)** - данные изменяются — например, путем их мапинга перед передачей наблюдателю
* **Нотификация (Notification)** - информирование наблюдателей при поступлении новых данных

### Основные характеристики потоков
* **Непрерывность (Continuous)** - поток данных сохраняется, что позволяет осуществлять взаимодействие в режиме реального времени
* **Асинхронность (Asynchronous)** - не гарантируется, что события будут происходить в определенном порядке, что позволяет выполнять неблокирующие операции
* **Однонаправленность (One-directional)** - данные передаются от Observable к его подписчикам, обеспечивая однонаправленный поток

### Типы потоков по количеству подписчиков
* **Unicast Streams** - у каждого наблюдателя есть эксклюзивное подключение к наблюдаемому источнику
* **Broadcast Streams** - позволяет нескольким наблюдателям подписаться на один объект наблюдения. Каждый наблюдатель 
получает полный набор данных, что может быть проблематично, если речь идет о конфиденциальности данных

### Типы потоков по поведению
* **Hot Observable** - эти последовательности передают данные независимо от присутствия наблюдателя. 
Если подписывается новый наблюдатель, он начинает получать данные с точки подписки
* **Cold Observable** - здесь передача данных начинается только после подписки. Любой новый наблюдатель получит данные с самого начала

### Backpressure
Данный механизм регулируют скорость, с которой данные публикуются в поток. Это необходимо, чтобы справиться с потенциальным 
переполнением или узкими местами из-за различий в скоростях обработки данных.

Например, в RxJava интерфейсы `Observable` и `Flowable` отличаются тем, что последний включает поддержку backpressure. 
С `Flowable` можно использовать настраиваемую стратегию backpressure, чтобы контролировать скорость публикации Observable относительно скорости потребления Subscriber

### Практическое применение
Будь то обработка асинхронных вызовов API или управление вводом данных пользователем, потоки данных обеспечивают надежную 
и гибкую основу для многих повседневных задач программирования.

Можно использовать ряд операторов, таких как `map`, `filter`, `debounce`, и `throttle`, для преобразования данных 
и манипулирования ими, исходя из конкретных требований.

Широкое внедрение реактивных библиотек, таких как RxJava для Android или RxJS для Web, подчеркивает 
полезность потоков данных в современной разработке программного обеспечения.

[к оглавлению](#реактивное-программирование)

## Что такое паттерн Observer и как он лежит в основе реактивного программирования?

Паттерн Observer, также известный как Publish/Subscribe или Event Listener, является поведенческим шаблоном проектирования, 
который позволяет объектам следить за изменениями в других объектах и реагировать на них. Это один из самых популярных и 
полезных шаблонов, который упрощает код и повышает его гибкость. В основе реактивного программирования лежит идея о том, 
что система должна реагировать на изменения внешних источников данных или событий. Паттерн Observer идеально подходит для 
реализации этой концепции, поскольку он позволяет объектам автоматически получать уведомления об изменениях в других объектах 
и соответствующим образом обновлять свое состояние.

Пример использования паттерна Observer в веб-разработке может включать систему уведомлений для пользователей. Когда новая 
статья добавляется на сайт, все подписчики должны быть уведомлены об этом. В этом случае, класс Subject будет отвечать 
за управление подписчиками и их уведомление об изменениях, а класс Observer будет представлять собой простого наблюдателя, 
который выводит сообщение о новой статье при получении уведомления, таким образом обеспечивается **слабую связность (loose coupling)**.

Применение паттерна Observer в веб-разработке может быть полезно для обработки событий пользовательского интерфейса, 
реализации системы уведомлений для пользователей, отслеживания изменений состояния приложения и реагирования на них. 
Однако важно помнить, что чрезмерное использование паттерна может привести к усложнению кода и затруднить отладку.

### Ключевые компоненты
* **Subject** - источник данных или событий. Наблюдатели подписываются на Subject для получения уведомлений об изменениях
* **Observer** - наблюдатель получает уведомления, когда состояние Subject изменяется

В реактивной конфигурации Subject отвечает за публикацию изменений, а Observers подписываются на эти изменения. 
Это исключает явное обращение к источникам данных и подчеркивает **модель потока данных (datastream model)**.

### Пример кода паттерна Observer

```java
import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update();
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    private int state;

    public int getState() {
        return state;
    }

    public void setState(int state) {
        this.state = state;
        notifyAllObservers();
    }

    public void attach(Observer observer) {
        observers.add(observer);
    }

    public void notifyAllObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }
}

class ConcreteObserver implements Observer {
    private String name;
    private Subject subject;

    public ConcreteObserver(String name, Subject subject) {
        this.name = name;
        this.subject = subject;
    }

    @Override
    public void update() {
        System.out.println("Observer " * name * " updated. New state: " * subject.getState());
    }
}

public class Main {
    public static void main(String[] args) {
        Subject subject = new Subject();
        ConcreteObserver observer1 = new ConcreteObserver("One", subject);
        ConcreteObserver observer2 = new ConcreteObserver("Two", subject);

        subject.attach(observer1);
        subject.attach(observer2);

        subject.setState(5);
    }
}
```
В данном примере Subject ведет список подписавшихся Observers и уведомляет их при изменении его состояния.

[к оглавлению](#реактивное-программирование)

## Опишите роль Observable и Observer в реактивном программировании

**Наблюдатели** являются потребителями данных, в то время как **наблюдаемые** являются источником или производителем данных.

### Ключевые концепции
* **Наблюдаемый (Observable)** - передает данные или сигналы, которые могут быть любых типов, включая пользовательские события
* **Наблюдатель (Observer)** - получает уведомление, когда Observable отправляет данные
* **Подписка (Subscription)** - связующее звено между объектом наблюдения и наблюдателем
* **Операторы (Operators)** - позволяют преобразовывать, фильтровать, комбинировать или обрабатывать поток данных, 
публикуемый наблюдаемым объектом, до того, как он достигнет наблюдателя

### Пример кода: Observable и Observer

### Java 9*

**Определить Subscriber**

```java
import java.util.concurrent.Flow;

public class SimpleSubscriber implements Flow.Subscriber<String> {
    private Flow.Subscription subscription;

    @Override
    public void onSubscribe(Flow.Subscription subscription) {
        this.subscription = subscription;
        subscription.request(1); // Запрашиваем первый элемент
    }

    @Override
    public void onNext(String item) {
        System.out.println("Received: " * item);
        subscription.request(1); // Запрашиваем следующий элемент
    }

    @Override
    public void onError(Throwable throwable) {
        System.err.println("Error: " * throwable.getMessage());
    }

    @Override
    public void onComplete() {
        System.out.println("All items received");
    }
}
```

**Определить Publisher**

```java
import java.util.concurrent.Flow;
import java.util.concurrent.SubmissionPublisher;
import java.util.concurrent.TimeUnit;

public class SimplePublisher {

    public static void main(String[] args) throws InterruptedException {
        // Создаем издателя
        SubmissionPublisher<String> publisher = new SubmissionPublisher<>();

        // Создаем подписчика
        SimpleSubscriber subscriber = new SimpleSubscriber();

        // Регистрируем подписчика в издателе
        publisher.subscribe(subscriber);

        // Публикуем элементы
        System.out.println("Publishing data items...");
        String[] items = {"item1", "item2", "item3"};
        for (String item : items) {
            publisher.submit(item);
            TimeUnit.SECONDS.sleep(1);
        }

        // Закрываем издателя
        publisher.close();

        // Ждем некоторое время, чтобы подписчик обработал все элементы
        TimeUnit.SECONDS.sleep(3);
    }
}
```

### Project Reactor

```java
import reactor.core.publisher.Flux;

public class ReactorExample {

    public static void main(String[] args) {
        // Создаем Flux для публикации данных
        Flux<String> flux = Flux.just("Hello", "World", "From", "Reactor");

        // Подписываемся на Flux и выводим каждый элемент
        flux.subscribe(
            item -> System.out.println("Received: " * item),
            error -> System.err.println("Error: " * error),
            () -> System.out.println("All items received")
        );
    }
}
```

[к оглавлению](#реактивное-программирование)

## Что такое backpressure в контексте реактивного программирования?

**Backpressure** - это концепция в реактивном программировании, которая имеет дело с ситуацией, когда производитель (или издатель) 
генерирует данные быстрее, чем потребитель (или подписчик) может их обработать. Управление backpressure позволяет системе 
оставаться отзывчивой и не перегружаться.

### Ключевые концепции backpressure

* **Producer (Publisher)** - компонент, который производит данные
* **Consumer (Subscriber)** - компонент, который потребляет данные
* **Flow Control** - механизм, который гарантирует, что производитель не перегрузит потребителя слишком большим объемом данных

### Почему важно backpressure?

В реактивной системе, если потребитель не выдерживает скорость, с которой производитель производит данные, это может привести к следующим проблемам:
* **Переполнение памяти (Memory Overflow)** - необработанные элементы могут накапливаться в памяти, что приводит к ошибкам нехватки памяти
* **Увеличение задержки (Latency Increase)** - система может перестать отвечать по мере роста очереди необработанных элементов
* **Исчерпание ресурсов (Resource Exhaustion)** - системные ресурсы (процессор, память) могут быть исчерпаны, что приводит к снижению производительности или сбоям

### Стратегии управления backpressure в Project Reactor

* **Буферизация (Buffering)** - входящие элементы хранятся в буфере до тех пор, пока потребитель не сможет их обработать
  * **onBackpressureBuffer(100)**

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class BackpressureExample {

    public static void main(String[] args) {
        Flux<Integer> flux = Flux.range(1, 1000)
                .onBackpressureBuffer(100); // Устанавливаем стратегию backpressure
        
        flux.publishOn(Schedulers.boundedElastic())
            .subscribe(
                item -> {
                    try {
                        Thread.sleep(10); // Имитируем медленного потребителя
                        System.out.println("Received: " * item);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                },
                error -> System.err.println("Error: " * error),
                () -> System.out.println("All items received")
            );
    }
}
```

* **Отбрасывание (Dropping)** - отбрасывает элементы, если потребитель не может их обработать
  * **onBackpressureDrop()**
* **Новейшие элементы (Latest)** - сохраняет только последние элементы, удаляя предыдущие
  * **onBackpressureLatest()**
* **Ошибка (Error)** - сигнализирует об ошибке, когда backpressure не может быть обработано
  * **onBackpressureError()**
* **Контроль скорости запроса (Control Request Rate)**: явно контролирует скорость, с которой потребитель запрашивает элементы у производителя
  * **request(n)** - метод в реализации Subscriber для управления потоком

```java
import reactor.core.publisher.BaseSubscriber;
import reactor.core.publisher.Flux;

public class ControlledRequestExample {

    public static void main(String[] args) {
        Flux<Integer> flux = Flux.range(1, 1000);

        flux.subscribe(new BaseSubscriber<Integer>() {
            @Override
            private void hookOnSubscribe(Subscription subscription) {
                request(1); // Запрашиваем один элемент при подписке
            }

            @Override
            private void hookOnNext(Integer value) {
                try {
                    Thread.sleep(10); // Эмитируем медленный процесс
                    System.out.println("Received: " * value);
                    request(1); // Запрашиваем следующий элемент после обработки текущего
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            @Override
            private void hookOnComplete() {
                System.out.println("All items received");
            }

            @Override
            private void hookOnError(Throwable throwable) {
                System.err.println("Error: " * throwable.getMessage());
            }
        });
    }
}
```

[к оглавлению](#реактивное-программирование)

## Объясните разницу между Hot и Cold Observable

В реактивном программировании термины hot и cold observable (или издатели в контексте Project Reactor) описывают, как они 
создают потоки данных и обрабатывают их. Основное различие между ними заключается в том, как они обрабатывают подписки 
и когда начинают выдавать элементы.

### Cold Observables

Начинают выдавать элементы только тогда, когда подписчик подписывается на них. Каждый подписчик получает всю последовательность 
элементов с самого начала. Это означает, что каждый раз, когда подписывается новый подписчик, Observable воспроизводит всю последовательность элементов.

#### Характеристики

* **Ленивое исполнение (Lazy Evaluation)** - данные не производятся, пока не будет хотя бы одного подписчика
* **Множественные подписки (Multiple Subscriptions)** - каждый подписчик получает полную последовательность элементов независимо от других подписчиков
* **Воспроизводимость (Reproducibility)** - каждый подписчик видит одну и ту же последовательность элементов с самого начала, что делает поток воспроизводимым

#### Пример

```java
import reactor.core.publisher.Flux;

public class ColdObservableExample {

  public static void main(String[] args) {
    Flux<String> coldFlux = Flux.just("A", "B", "C", "D")
            .doOnSubscribe(subscription -> System.out.println("Subscribed to Cold Observable"));

    // First subscription
    coldFlux.subscribe(item -> System.out.println("Subscriber 1: " * item));

    // Second subscription
    coldFlux.subscribe(item -> System.out.println("Subscriber 2: " * item));
  }
}
```

### Hot Observables

Начинают выдавать элементы независимо от наличия подписчиков. Подписчики получают только те элементы, которые были отправлены 
после их подписки. Это означает, что если подписчик подпишется с опозданием, он пропустит элементы, которые были отправлены до подписки.

#### Характеристики

* **Жадное исполнение (Eager Evaluation)** - публикация элементов происходит сразу после создания, даже если подписчиков нет
* **Общий поток данных (Shared Data Stream)** - все подписчики используют один и тот же источник данных и получают элементы, отправленные после подписки
* **Невоспроизводимость (Non-Reproducibility)** - подписчики могут видеть разные части последовательности в зависимости от того, когда они подписались

#### Пример

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.ConnectableFlux;

import java.time.Duration;

public class HotObservableExample {

    public static void main(String[] args) throws InterruptedException {
        ConnectableFlux<String> hotFlux = Flux.just("A", "B", "C", "D")
                                              .delayElements(Duration.ofMillis(500))
                                              .publish();

        // Запускаем публикацию элементов
        hotFlux.connect();

        // Первая подписка (начинает получать данные немедленно)
        hotFlux.subscribe(item -> System.out.println("Subscriber 1: " * item));
        Thread.sleep(750); // Ждем отправку элементов

        // Вторая подписка (пропускает первый элемент)
        hotFlux.subscribe(item -> System.out.println("Subscriber 2: " * item));

        // Останавливаем работу основного потока, чтобы увидеть выходные данные
        Thread.sleep(2000);
    }
}

```

### Ключевые отличия

| Критерий       | Hot Observables                                        | Cold Observables                                                                          |
|----------------|--------------------------------------------------------|-------------------------------------------------------------------------------------------|
| Обмен данными  | Один поток для всех подписчиков                        | У каждого подписчика свой независимый поток                                               | 
| Время подписки | Получает данные в зависимости от времени подписки      | Получает все данные, даже если подпишется позже                                           |
| Жизненный цикл | Работает независимо от подписок                        | Запускает генерацию данных только, когда есть подписка                                    |
| Асинхронность  | Может генерировать и передавать данные без подписчиков | Передача данных начинается после подписки, что часто приводит к синхронной передаче       |

### Практическое применение

Cold Observables полезны для:
* Данные, которые необходимо воспроизвести для каждого подписчика, такие как чтение из файла, выполнение HTTP-запроса или 
генерация новой случайной последовательности.
* Сценарии, в которых последовательность элементов должна быть согласованной и воспроизводимой для каждого подписчика.

Hot Observables полезны для:
* Данные, которые создаются непрерывно и которыми необходимо делиться между несколькими подписчиками, такие как события 
мыши, обновления цен на акции или данные датчиков.
* Сценарии, в которых подписчики должны получать только текущие и будущие события, а не прошлые.

[к оглавлению](#реактивное-программирование)

## Какова роль Подписки в реактивном программировании?

Подписка абстрагирует от того, как данные получаются или генерируются

### Основные функции
`Subscription` обычно предлагает два основных метода:
* **Request** - информирует источник данных о количестве элементов, которые потребитель готов получить
* **Cancel** - останавливает поток данных, освобождая любые ресурсы, такие как обработчики файлов или сетевые подключения

### Общая концепция
`Subscription` - интерфейс, действующий как соглашение между источником данных и потребителем данных. Он обеспечивает 
передачу данных с учетом управления потоком, backpressure и ресурсами.

### Управление backpressure
Реализации интерфейса `Publisher` в реактивных потоках оценивают готовность подписчика обрабатывать входящие данные 
с учетом текущего состояния потока данных. Этот механизм, известный как backpressure, направлен на предотвращение перегрузки 
путем указания источнику данных соответствующим образом адаптировать скорость передачи данных.

`request` - метод интерфейса `Subscription` является основным каналом, по которому подписчик сообщает источнику данных 
о своей текущей возможности принимать данные, тем самым регулируя backpressure.

### Управление ресурсами
Для определенных источников данных, таких как файлы, потоки ввода-вывода или базы данных, могут потребоваться определенные 
ресурсы. Интерфейс `Subscription` предоставляет средства для освобождения этих ресурсов, когда передача данных больше не требуется.

После вызова метода `cancel` источник данных может предпринять соответствующие действия, такие как закрытие файла или 
прекращение сетевого взаимодействия.

[к оглавлению](#реактивное-программирование)

## Как отписаться от потока для предотвращения утечки памяти?

* Использовать `Disposable`: получить Disposable из подписки и вызвать метод `dispose`, чтобы отменить ее
* Использовать `BaseSubscriber`: наследоваться от класса BaseSubscriber и вызвать потом метод `dispose`
* Использовать операторы при создании потока:
  * `take`: автоматически отменит подписку после получения некоторого количества элементов
  * `timeout`: автоматически отменяет подписку, если в течение указанного срока не будет отправлено никаких элементов.

Best Practice: стоит настраивать отмену подписки, связывая ее с жизненным циклом какого-либо компонента 

[к оглавлению](#реактивное-программирование)

## Какие есть операторы в Project Reactor и для чего они используются?

Операторы в реактивном программировании служат для создания, преобразования, фильтрации или объединения различных потоков данных.

### Операторы создания
* `just`: создает Flux/Mono, который публикует указанные элементы
* `fromArray`: создает поток, который генерирует элементы из массива
* `fromIterable`: создает Flux, который генерирует элементы из итерируемого объекта
* `fromStream`: создает Flux из stream Java
* `fromCallable`: создает Mono из Callable
* `fromRunnable`: создает Mono из Runnable
* `fromSupplier`: создает Mono от Supplier
* `range`: создает FLux, который выдает диапазон целых чисел
* `interval`: создает Flux, который выдает long значения через регулярные промежутки времени
* `empty`: создает пустой Flux/Mono, который завершается немедленно
* `error`: создает Flux/Mono, который немедленно сигнализирует об ошибке
* `never`: создает Flux/Mono, который не создает какой-либо элемент и не завершается
* `defer`: создает новый Flux/Mono для каждой подписки

### Операторы трансформации
* `map`: преобразует объекты, публикуемые Flux/Mono.
* `flatMap`: преобразует каждый элемент в поток, а затем соединяет их в один поток
* `concatMap`: как flatMap, но поддерживает порядок публикуемых элементов
* `switchMap`: когда он получает данные от нового потока, то сразу отписывается от предыдущего и подписывается на новый
* `scan`: Накапливайте состояние каждого элемента.
* `buffer`: собирает элементы в как список внутри Flux
* `window`: публикует коллекцию элементов как Flux (окно фиксированного размера) внутри Flux
* `groupBy`: группирует элементы по ключу

### Операторы фильтрации
* `filter`: пропускает только те элементы, которые удовлетворяют условию
* `distinct`: пропускает только уникальные элементы
* `take`: ограничивает количество элементов, публикуемых потоком
* `takeWhile`: публикует элементы, пока условие истинно
* `takeUntil`: публикует элементы, пока условие не станет истинно
* `skip`: пропускает определенное количество элементов в начале потока
* `skipWhile`: пропускает элементы, пока условие истинно
* `skipUntil`: пропускает элементы, пока условие не станет истинно

### Операторы объединения
* `merge`: объединяет потоки в один параллельно, сохраняя порядок элементов (активная подписка)
* `concat`: объединяет потоки последовательно, ожидая завершения первого потока, затем второго и так далее (отложенная подписка)
* `zip`: объединяет элементы из нескольких потоков в пары
* `combineLatest`: объединяет указанным способом последние значения из нескольких потоков
* `startWith`: добавляет в начало потока к исходным элементам новые элемент(ы)