# AddPercentDamageDecorator


**1. Основная идея:**

*   Декоратор будет оборачивать другой объект, который уже имеет некоторый базовый урон.
*   Декоратор будет увеличивать этот базовый урон на заданный процент.
*   Это позволит динамически добавлять урон без изменения исходного объекта.

**2. Пример реализации (C#):**

```csharp
// Базовый интерфейс для объектов, которые могут наносить урон
public interface IDamageable
{
    int GetDamage();
}

// Конкретный класс, реализующий IDamageable (например, оружие)
public class Weapon : IDamageable
{
    public int BaseDamage { get; set; }

    public Weapon(int baseDamage)
    {
        BaseDamage = baseDamage;
    }

    public int GetDamage()
    {
        return BaseDamage;
    }
}

// Абстрактный класс декоратора
public abstract class DamageDecorator : IDamageable
{
    protected IDamageable _damageable;

    public DamageDecorator(IDamageable damageable)
    {
        _damageable = damageable;
    }

    public abstract int GetDamage();
}

// Класс AddPercentDamageDecorator
public class AddPercentDamageDecorator : DamageDecorator
{
    private double _percentIncrease;

    public AddPercentDamageDecorator(IDamageable damageable, double percentIncrease) : base(damageable)
    {
        _percentIncrease = percentIncrease;
    }

    public override int GetDamage()
    {
        int baseDamage = _damageable.GetDamage();
        double increaseAmount = baseDamage * _percentIncrease;
        return baseDamage + (int)Math.Round(increaseAmount);
    }
}

// Пример использования
public class Program
{
    public static void Main(string[] args)
    {
        // Создаем оружие с базовым уроном 10
        Weapon sword = new Weapon(10);
        Console.WriteLine($"Sword damage: {sword.GetDamage()}"); // Output: Sword damage: 10

        // Добавляем 50% урона к мечу
        AddPercentDamageDecorator enhancedSword = new AddPercentDamageDecorator(sword, 0.5);
        Console.WriteLine($"Enhanced sword damage: {enhancedSword.GetDamage()}"); // Output: Enhanced sword damage: 15

        // Создаем еще один декоратор, увеличивающий урон на 20% от уже увеличенного урона
        AddPercentDamageDecorator superEnhancedSword = new AddPercentDamageDecorator(enhancedSword, 0.2);
        Console.WriteLine($"Super enhanced sword damage: {superEnhancedSword.GetDamage()}");  // Output: Super enhanced sword damage: 18

        Console.ReadKey();
    }
}
```

**3. Объяснение кода:**

*   **`IDamageable`:** Интерфейс, который определяет, что объект может наносить урон.
*   **`Weapon`:**  Конкретный класс, реализующий `IDamageable` и представляющий оружие с базовым уроном.
*   **`DamageDecorator`:** Абстрактный класс, от которого наследуются все декораторы урона. Он содержит ссылку на объект `IDamageable`, который он оборачивает.
*   **`AddPercentDamageDecorator`:** Конкретный класс декоратора, который добавляет процентный урон.  Он принимает объект `IDamageable` и процент увеличения урона в конструкторе.  В методе `GetDamage()` он получает базовый урон от обернутого объекта и увеличивает его на заданный процент.
*   **`Program`:** Пример использования.  Создает оружие, оборачивает его декораторами и выводит урон.

**4. Ключевые моменты:**

*   **Интерфейс `IDamageable`:** Позволяет применять декораторы к различным типам объектов, которые могут наносить урон (не только к оружию).
*   **Абстрактный класс `DamageDecorator`:** Упрощает создание новых декораторов.
*   **Процентное увеличение урона:**  Хранится в переменной `_percentIncrease` в классе `AddPercentDamageDecorator`.
*   **Метод `GetDamage()`:**  Вычисляет увеличенный урон.

**5. Преимущества использования Decorator Pattern:**

*   **Динамическое добавление функциональности:**  Можно добавлять урон во время выполнения программы, не изменяя исходный класс `Weapon`.
*   **Гибкость:**  Можно создавать различные комбинации декораторов для достижения нужного эффекта.
*   **Избежание раздувания классов:**  Не нужно добавлять много кода в класс `Weapon` для обработки разных типов урона.  Вместо этого используются декораторы.

