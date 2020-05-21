### 工厂方法

定义一个创建对象的接口，让其子类决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。

### 优点

一个调用者想创建一个对象，只要知道其名称就可以了。扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。屏蔽产品的具体实现，调用者只关心产品的接口。

### 缺点

每次增加一个产品时，都需要增加一个具体的类，并修改对象实现工厂，使得系统中类的个数成倍增加，增加了系统的复杂度。

### 使用场景

1、日志记录器：记录在文件系统，redis，数据库，云端等。2、数据库访问。

### 实例

```php
<?php
// 产品接口
interface Shape{
	public function draw();
}

// 产品类1
class Rectangle implements Shape{

	public function draw(){
		echo "执行：Rectangle::draw()方法<br>";
	}
}

// 产品类2
class Square implements Shape{
	public function draw(){
		echo "执行：Square::draw()方法<br>";
	}
}

// 产品类3
class Circle implements Shape{
	public function draw(){
		echo "执行：Circle::draw()方法<br>";
	}
}


// 创建一个工厂
class ShapeFactory{
	public function getShape($shapeType){
		if($shapeType == null){
			return null;
		}

		if($shapeType == "CIRCLE"){
			return new Circle();
		} else if($shapeType == "RECTANGLE"){
			return new Rectangle();
		}else if($shapeType == "SQUARE"){
			return new Square();
		}

		return null;
	}
}

// 使用该工厂，通过传递类型来获取实体类对象
$shapeFactory = new ShapeFactory();

$shape1 = $shapeFactory->getShape("CIRCLE");
$shape1->draw();
$shape2 = $shapeFactory->getShape("RECTANGLE");
$shape2->draw();
$shape3 = $shapeFactory->getShape("SQUARE");
$shape4->draw();

```

执行结果：

```
执行：Circle::draw()方法
执行：Rectangle::draw()方法
执行：Square::draw()方法
```