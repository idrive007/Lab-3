# Звіт з лабораторної роботи №3. Оператори switch.
> Виконала студентка групи ІКМ-М223в **Павленко Дарина**
### Мета: Рефакторінг коду з збитковою кількістю операторів switch.

### Завдання 1: Наданий код
    
    class Shape:
      def __init__(self, shape_type):
          self.shape_type = shape_type

      def calculate_area(self):
          area = 0
          if self.shape_type == "circle":
              radius = float(input("Enter the radius of the circle: "))
              area = 3.14 * radius * radius
          elif self.shape_type == "rectangle":
              length = float(input("Enter the length of the rectangle: "))
              width = float(input("Enter the width of the rectangle: "))
              area = length * width
          elif self.shape_type == "triangle":
              base = float(input("Enter the base of the triangle: "))
              height = float(input("Enter the height of the triangle: "))
              area = 0.5 * base * height
          print("Area:", area)

    # Приклад виклику програми
    shape_type = input("Enter the type of shape (circle, rectangle, triangle): ")
    shape = Shape(shape_type)
    shape.calculate_area()

### 1.1 Аналіз

У класі Shape використовується оператор if-elif-else, щоб вибрати правильну формулу для обчислення площі залежно від типу фігури. Це призводить до довгого списку умов, що може бути важко підтримувати та розширювати в майбутньому, особливо якщо потрібно додати нові типи фігур.
Окрім того, в кожному блоку if-elif-else є дублювання коду для отримання введених користувачем значень, що призводить до великого обсягу коду.
Метод calculate_area виконує як обчислення площі, так і вивід результату на екран. Це зводиться до змішування обчислювального та представницького коду, що може ускладнити тестування та рефакторинг.

### 2.1 Рішення

    class Shape:
        def calculate_area(self):
          pass

    class Circle(Shape):
        def __init__(self, radius):
            self.radius = radius

        def calculate_area(self):
            return 3.14 * self.radius * self.radius

    class Rectangle(Shape):
        def __init__(self, length, width):
            self.length = length
            self.width = width

        def calculate_area(self):
            return self.length * self.width

    class Triangle(Shape):
        def __init__(self, base, height):
            self.base = base
            self.height = height

        def calculate_area(self):
            return 0.5 * self.base * self.height

    def main():
        shape_type = input("Enter the type of shape (circle, rectangle, triangle): ")

        if shape_type == "circle":
            radius = float(input("Enter the radius of the circle: "))
            shape = Circle(radius)
        elif shape_type == "rectangle":
            length = float(input("Enter the length of the rectangle: "))
            width = float(input("Enter the width of the rectangle: "))
            shape = Rectangle(length, width)
        elif shape_type == "triangle":
            base = float(input("Enter the base of the triangle: "))
            height = float(input("Enter the height of the triangle: "))
            shape = Triangle(base, height)
        else:
            print("Invalid shape type!")
            return

        area = shape.calculate_area()
        print("Area:", area)

    if __name__ == "__main__":
        main()

Новий код використовує об'єктно-орієнтований підхід, де кожна фігура є окремим класом з методом для обчислення площі. Це робить код більш читабельним, організованим.
Також тепер присутнє використання поліморфізму, де всі класи фігур успадковуються від базового класу Shape і мають метод calculate_area. Це дозволяє динамічно викликати правильний метод обчислення площі, не використовуючи довгі ланцюжки умов.
В коді також уникається дублювання коду, оскільки кожен клас фігури має свою власну реалізацію методу calculate_area, що спрощує підтримку та розширення.
Клас Shape тепер відповідає тільки за обчислення площі, а ввод та вивід даних реалізовані в функції main(), що полегшує розуміння та тестування коду.

### Завдання 2: Наданий код 

    class ShapeCalculator:
    def calculate_area(self, shape_type):
        if shape_type == "circle":
            self.calculate_circle_area()
        elif shape_type == "rectangle":
            self.calculate_rectangle_area()
        elif shape_type == "triangle":
            self.calculate_triangle_area()

    def calculate_circle_area(self):
        radius = float(input("Enter the radius of the circle: "))
        area = 3.14 * radius * radius
        print("Area:", area)

    def calculate_rectangle_area(self):
        length = float(input("Enter the length of the rectangle: "))
        width = float(input("Enter the width of the rectangle: "))
        area = length * width
        print("Area:", area)

    def calculate_triangle_area(self):
        base = float(input("Enter the base of the triangle: "))
        height = float(input("Enter the height of the triangle: "))
        area = 0.5 * base * height
        print("Area:", area)

    # Приклад використання класу
    calculator = ShapeCalculator()
    calculator.calculate_area("circle")

### 1.2 Аналіз 

Бачимо, що функція calculate_area використовує оператор switch для визначення типу фігури та виклику відповідного методу обчислення площі. 
У методі calculate_area є дублювання коду для виклику відповідних методів для кожного типу фігури. Це ускладнює зміну коду та підтримку.
Клас ShapeCalculator відповідає як за визначення типу фігури, так і за обчислення її площі. Це порушує принцип єдиного обов'язку та робить клас менш зручним для розширення.

### 2.2 Рішення

     class ShapeCalculator:
        def calculate_area(self, shape_type):
            shape = self.create_shape(shape_type)
            if shape:
                shape.calculate_area()

        def create_shape(self, shape_type):
            if shape_type == "circle":
                return Circle()
            elif shape_type == "rectangle":
                return Rectangle()
            elif shape_type == "triangle":
                return Triangle()
            else:
                print("Unknown shape type")
                return None

    class Circle:
        def calculate_area(self):
            radius = float(input("Enter the radius of the circle: "))
            area = 3.14 * radius * radius
            print("Area:", area)

    class Rectangle:
        def calculate_area(self):
            length = float(input("Enter the length of the rectangle: "))
            width = float(input("Enter the width of the rectangle: "))
            area = length * width
            print("Area:", area)

    class Triangle:
        def calculate_area(self):
            base = float(input("Enter the base of the triangle: "))
            height = float(input("Enter the height of the triangle: "))
            area = 0.5 * base * height
            print("Area:", area)

    # Приклад використання класу
    calculator = ShapeCalculator()
    calculator.calculate_area("circle")

Тепер логіка calculate_area розподілена між методом calculate_area та методом create_shape.
Змінений код використовує поліморфізм, де кожен клас фігури має метод calculate_area, що дозволяє легко додавати нові типи фігур без зміни коду в класі ShapeCalculator.
Також клас ShapeCalculator тепер відповідає тільки за визначення типу фігури та створення відповідного об'єкту, а обчислення площі відводиться кожному окремому класу фігури.
