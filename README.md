Tabii, işte bir Observer deseni örneği için C# projesi:

```csharp
using System;
using System.Collections.Generic;

// Observer arayüzü
public interface IObserver
{
    void Update(int value);
}

// İzlenecek nesne (Subject)
public class Subject
{
    private int _state;
    private List<IObserver> _observers = new List<IObserver>();

    public int State
    {
        get { return _state; }
        set
        {
            _state = value;
            NotifyObservers();
        }
    }

    public void Attach(IObserver observer)
    {
        _observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        _observers.Remove(observer);
    }

    private void NotifyObservers()
    {
        foreach (var observer in _observers)
        {
            observer.Update(_state);
        }
    }
}

// İzleyici sınıfı (Observer)
public class Observer : IObserver
{
    private string _name;

    public Observer(string name)
    {
        _name = name;
    }

    public void Update(int value)
    {
        Console.WriteLine($"Observer {_name} received update. New state: {value}");
    }
}

// Kullanım
class Program
{
    static void Main(string[] args)
    {
        // İzlenecek nesne oluşturulur
        var subject = new Subject();

        // İzleyiciler oluşturulur
        var observer1 = new Observer("Observer 1");
        var observer2 = new Observer("Observer 2");
        var observer3 = new Observer("Observer 3");

        // İzleyiciler izlenen nesneye eklenir
        subject.Attach(observer1);
        subject.Attach(observer2);
        subject.Attach(observer3);

        // İzlenecek nesnenin durumu güncellenir
        subject.State = 1;

        // Bir izleyiciyi kaldırma
        subject.Detach(observer2);

        // İzlenecek nesnenin durumu tekrar güncellenir
        subject.State = 2;

        Console.ReadLine();
    }
}
```

Bu örnekte, `Subject` sınıfı bir `State` özelliğine sahip ve bu özellik değiştirildiğinde `NotifyObservers` metodu çağrılarak tüm izleyicilere güncelleme bildirimi yapılır. `Observer` sınıfı ise `Update` metoduyla güncellemeleri alır ve ekrana yazdırır. `Main` metodunda örnek bir kullanım gösterilmiştir. `Subject` sınıfına bağlı olan izleyicilerin (observers) durumu güncellemeleri anında alır ve işlerini yaparlar.
