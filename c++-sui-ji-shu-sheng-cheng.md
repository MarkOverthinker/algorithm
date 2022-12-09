# C++随机数生成

```
// Some code
srand(种子);
srand((unsigned)time(NULL));
result = rand();
```

C++11提供的均匀分布模板类为：uniform\_int\_distribution和uniform\_real\_distribution。前一个模板类名字中的int不是代表整型，而是表示整数。因为它是一个模板类，可以用int、long、short等整数类型来实例化。后一个表示浮点数模板类，可以用float和double来实例化。

```
// 生成0-1之间的数（均匀分布）
mt19937 gen{random_device{}()};
uniform_real_distribution<double> dis(0, 1); //小数
uniform_int_distribution<int> dis1(0, 100); // 整数
result = dis(gen);
```
