Наступне описує обов'язкові вимоги які мають бути дотримані для сумісності автозавантаження.

Обов'язкові вимоги
---------
* Повністю зазначений простір імен та клас повинні мати наступну структуру  `\<Vendor Name>\(<Namespace>\)*<Class Name>`.
* Кожен простір імен повинен містити на початку ім'я вендора ("Vendor Name").
* Кожен простір імен може мати стільки під-простірів імен скільки потрібно. 
* Кожен розділитель простору імен конвуртуеться до `DIRECTORY_SEPARATOR` при завантаженні з файлової системи.
* Кожен символ `_` в імені класа конвертуеться до `DIRECTORY_SEPARATOR`. Символ `_` не має специфічного призначення у просторі імен.
* Повністю зазначений простір імен та клас повинні мати суфікс`.php` при завантаженні з файлової системи.
* Імена вендора, простори імен та імена класів можуть складати літери у будь-якій комбінації нижнього та верхнього регістру.

Приклади
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

Символ нижнього підкреслення у просторах імен та іменах класів
--------------------------------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

Стандарти які ми наводимо тут мають стати найменьшим спільним знаменником у болючому питанні сумісності автозавантаження.

Визначити чи дотримаютесь ви вищезазначенних умов можна викороставши цей зразок реалізації класа SplClassLoader, який має змогу завантажувати класи PHP 5.3.

Приклад Реалізації
------------------

Нижче наведен приклад, який легко показуе як працюе автозавантаження за наведених вище умов.

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

Реалізація класа SplClassLoader 
-------------------------------

Нижче наведений лінк на гіст - це зразок реалізації класа SplClassLoader, який здатен завантажувати класи, якщо дотримані вищезазначені вимои. 

На цей час це поточні рекомендації щодо загрузки класів PHP 5.3, в яких дотримані ці стандарти.

* [http://gist.github.com/221634](http://gist.github.com/221634)

