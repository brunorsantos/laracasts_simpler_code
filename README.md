# laracasts_simpler_code

## No abreviations

Em nenhuma circunstancia utilizar abreviações em nenhuma dessa situações:

Nomes de variáveis
```php
foreach($x in people){

}
```
Nomes de classes
```php
class repo{}
```
Nomes de metodos
```php
class Repository{

    public function sh()
}
```

Ser o mais claro possível na nomeclatura dos métodos, o caso a seguir
```php
class UserRepository{

    public function fetch($billingId){}
}
```
Não é adequado por se especificar a que metodo 'fetch' se refere a uma 'billing'. (Mesmo o parametro sendo uma 'billingId'). O correto seria:
```php
class UserRepository{

    public function fetchByBillingId($billingId){}
}
```

Tentar utilizar sempre duas palavras no nome do método, caso esteja ocorrendo dificuldade em seguir esta regra, pode significar que o metódo possui muitas reponsabilidades. Como em
```php
class Order{

    public function prepAndShipAndNotifyUser($billingId){}
}
```

Não ser específico demais como em:
```php
class Order{

    public function shipOrder($billingId){}
}
```




## Don't use Else

Nos métodos, testar o caso dos possíveis erros primeiro, sendo assim o 'if' encontra o possivel erro e efetua o 'return' sem necessidade de else
```php

public function store($req)
{
	$validation = Validator::make($input,['username' => 'required']);
	
	if ($validation->fails())
	{
		return Redirect::back()->wihtInput()->withErrors($validation);
	}
}
```

Utilizar classes interfaces para definir tipos de instancias diferentes. Assim tambem se evita comparação de tipos diretamente com 'if'.


## One level of Indetation

## Wrap Primitives