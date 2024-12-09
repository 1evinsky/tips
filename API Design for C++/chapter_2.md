
# Розділ 2: Якісні характеристики API

## 2.1 Моделювання проблемної області
API має відображати реальні об’єкти та операції.  
- **Сенсова відповідність:** Імена та структура API повинні бути інтуїтивно зрозумілими.  
- **Абстракція:** Відокремлюйте складну логіку.  

**Приклад:**  
```cpp
class Image {
public:
    void Load(const std::string& filePath);
    void Resize(int width, int height);
    void Save(const std::string& outputFile);
};
```
Об’єкт `Image` інтуїтивно представляє роботу з зображеннями.

---

## 2.2 Приховування деталей реалізації
- **Фізичне приховування:** Оголошуйте інтерфейс у `.h`, а реалізацію у `.cpp`.  
- **Pimpl-ідіома:** Приховує реалізацію через вказівник на прихований клас.  

**Приклад:**  
```cpp
// Header
class API {
public:
    API();
    ~API();
    void DoSomething();
private:
    class Impl;
    Impl* pImpl;  // Вказівник на прихований клас
};
```

```cpp
// Implementation
class API::Impl {
public:
    void DoSomething() { /* Реалізація */ }
};

API::API() : pImpl(new Impl()) {}
API::~API() { delete pImpl; }
void API::DoSomething() { pImpl->DoSomething(); }
```

---

## 2.3 Мінімальна повнота
API має бути простим та функціонально завершеним.  
- **Не дублюйте функціональність.**  
- **Додавайте тільки необхідні методи.**

**TIP:** Уникайте дублювання методів. Використовуйте шаблони та опції.

**Приклад:**  
```cpp
void OpenFile(const std::string& path, bool readOnly = true); // Замість двох методів
```

---

## 2.4 Простота використання
- **Інтуїтивність:** Імена функцій та параметрів мають бути зрозумілими.  
- **Запобігання помилкам:** Створюйте API так, щоб ним було важко скористатись неправильно.  

**Приклад поганого API:**  
```cpp
void Configure(bool enable, bool reset = false);
```
**Кращий варіант:**  
```cpp
void EnableFeature();
void ResetConfiguration();
```

---

## 2.5 Слабке зв'язування
- Мінімізуйте залежності між модулями API.  
- Використовуйте **callback-функції** або **шаблони**.

**Приклад:**  
```cpp
class Observer {
public:
    virtual void OnEvent() = 0;
};

class Subject {
public:
    void AddObserver(Observer* obs);
    void Notify();
};
```

---

## 2.6 Стабільність, документація та тестування
- **Стабільність:** Уникайте змін, які руйнують сумісність.  
- **Документація:** Приклади використання API повинні бути обов'язковими.  
- **Тестування:** Пишіть юніт-тести для всіх функцій API.  

**TIP:** "Документація повинна бути настільки ж важливою, як і код API."

**Приклад:**  
```cpp
/// @brief Opens a file for reading.
/// @param filePath Path to the file.
/// @return True if successful.
bool OpenFile(const std::string& filePath);
```
