### 观察者模式

很常见的设计模式，有利于解耦，使用时要避免出现互相观察（死循环）

```php

<?php

/**
 * 观察目标类
 */
class Subject 
{
	private $observers = [];
	private $state;

	//注册观察者
	public function attach($name, Observer $observer)
	{
		$this->observers[$name] = $observer;
	}

	//state改变时，对观察者进行通知
	public function setState($state = 0)
	{
		$this->state = $state;
		$this->notify();
	}

	public function getState()
	{
		return $this->state;
	}

	//通知观察者执行相应操作
	public function notify()
	{
		if(empty($this->observers)){
			return;
		}

		foreach ($this->observers as $key => $observer) {
			$observer->sendMsg();
		}
	}
}

/**
 * 观察者接口
 */
interface Observer
{
	public function __construct(Subject $subject);
	public function sendMsg();
}

/**
 * 观察者1
 */
class MailObserver implements Observer
{
	private $subject;
	public function __construct(Subject $subject)
	{
		$this->subject = $subject;
		$this->subject->attach('mail',$this);
	}
	public function sendMsg()
	{
		echo 'send a mail '.$this->subject->getState()."<br>";
	}
}
 /**
 * 观察者2
 */
class MessageObserver implements Observer
{
	private $subject;
	public function __construct(Subject $subject)
	{
		$this->subject = $subject;
		$this->subject->attach('message',$this);
	}

	public function sendMsg()
	{
		echo 'send a message '.$this->subject->getState()."<br>";
	}
}
// ------------------------
// 以下为测试代码
// ------------------------
$subject = new Subject();

$obs1 = new MailObserver($subject);
$obs2 = new MessageObserver($subject);

echo "第一次setState: <br>";
$subject->setState(1);

echo "第二次setState: <br>";
$subject->setState(2);

?>
```
### 结果

	第一次setState:
	send a mail 1
	send a message 1
	第二次setState:
	send a mail 2
	send a message 2
