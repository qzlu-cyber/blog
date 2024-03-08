### static

1. 静态变量（Static Variables）：

   - 静态变量在程序的整个生命周期中保持其值不变。
   - 静态变量只在其所在的函数、方法或类的作用域内可见。
   - 静态变量在函数、方法或类的不同实例之间共享。
   - 静态变量的生存期从程序开始到程序结束。

   ```cpp
   #include <iostream>
   
   void increment()
   {
      static int count = 0; // 静态变量
      count++;
      std::cout << "Count: " << count << std::endl;
   }
   
   int main()
   {
      increment(); // 输出 Count: 1
      increment(); // 输出 Count: 2
      increment(); // 输出 Count: 3
      return 0;
   }
   ```

2. 静态函数（Static Functions）：

   - 静态函数属于类，不依赖于类的任何实例。
   - 静态函数不能直接访问非静态成员变量，只能访问静态成员变量和其他静态函数。
   - 静态函数可以通过类名来调用，也可以通过对象来调用。

   ```cpp
   #include <iostream>
   
   class MathUtils
   {
   public:
      static int add(int a, int b) // 静态函数
      {
          return a + b;
      }
   };
   
   int main()
   {
      int sum = MathUtils::add(5, 3); // 通过类名调用静态函数
      std::cout << "Sum: " << sum << std::endl; // 输出 Sum: 8
   
      MathUtils math;
      sum = math.add(2, 7); // 通过对象调用静态函数
      std::cout << "Sum: " << sum << std::endl; // 输出 Sum: 9
   
      return 0;
   }
   ```

3. 静态成员变量（Static Member Variables）：

   - 静态成员变量属于类，而不是类的实例。
   - 所有类的实例共享相同的静态成员变量。
   - 静态成员变量可以通过类名和对象访问。
   - 只能在类的声明中进行声明，不能在函数内部进行声明，必须在类的定义外（类的实现中）定义，静态成员变量的定义将为其分配内存空间。

   ```cpp
   #include <iostream>
   
   class Circle
   {
   private:
      static float pi; // 静态成员变量
      int radius;
   
   public:
      Circle(int r) : radius(r) {}
   
      float getArea()
      {
          return pi * radius * radius;
      }
   };
   
   float Circle::pi = 3.14; // 初始化静态成员变量
   
   int main()
   {
      Circle c1(5);
      Circle c2(3);
   
      std::cout << "Area of c1: " << c1.getArea() << std::endl; // 输出 Area of c1: 78.5
      std::cout << "Area of c2: " << c2.getArea() << std::endl; // 输出 Area of c2: 28.26
   
      return 0;
   }
   ```

   