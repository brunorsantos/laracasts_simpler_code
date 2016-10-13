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

Tentar o maximo utilizar apenas um nivel de identação por metodo.(Desconsiderando a identação inicial)

Atentar para ninhos de if que podem ser utilizados 'and' como:
```php
if($account->type()== 'Savings' && $account->isActive()){
 //Codigo identado
}
```

Alem disso a verificação acima por se tratar de uma regra de negocio especifica (conta para ser considerado poupança, deve tambem estar ativa), pode ser incluida em um metodo dentro da própria classe 'Account'. Evitando assim a identação do 'IF'.

EVitar ao maximo foreach iterando em um array e fazendo um verificao para retornar um novo array, utilizar a funcao array_filter(). Utilizando desta forma, conseguimos uma unica identação no método:

```php
funcion filterBy()
{
    return array_map($this->acconuts, function($account) use ($accountType)
    {
        return $account->isOfType($accountType);
    });
}
```






## Limit Your Instance Variables

Tentar limtar os atributos dentro da sua classe que são outros objetos. (Via dendency injection).
Utilizar no maximo 4 ou 5.

Caso com atributos demais
```php
class UserController{
    // atributos declarados
    ...
    
    public funcion __construct(
        UserService $userService,
        RegistrationService $registrationService,
        UserRepository $userRepository,
        Stripe $stripe, //Cobrança para resigtro
        Mailer $mailer,
        UserEventRepository $userEventRepository,
        Logger $logger
        
    ){
    // Atribuições aqui
    }
  
}
```
Verificar casos em que as funcionalidades poderiam estar em uma unica classe, como 
'UserService' e 'UserRepository' 'UserEventRepository' e , o que signifaria que o UserService quem faria os acessos ao repositorio.

```php
class UserService{
    // atributos declarados
    ...
    
    public funcion __construct(UserRepository $userRepository, UserEventRepository)
    {
    // Atribuições aqui
    }
  
}
```

Outra possiblidade é considerar que a sua classe poderia ter algumas funcionalidades removidas dela. E possivelmente criando outra classe. o objeto da clase Stripe (Boleto). Poderia esta incluido em outra controller como 'AuthController' que teriam metodos referentes a cancelar e criar uma conta, contendo nele a dependencia do boleto.

## Wrap Primitives

Com cuidado, deve ser verificar se um tipo primitivo pode ter uma classe criada para ele baseado nos seguintes fatores: (Obedecer a todos)

* Está trazendo clareza
```php
funcion cache($data, Second $seconds)
{

}

cache([] new Second(50))
```

* Existe comportamento associado ao attributo?

Exemplos de comportamento são, aumento dos segundo, aumento de peso.

* Existe validação associada ao atributo? (Consistencia)

Verificar se um email é valido por exemplo, deixando a propria classe responsável pela sua consistencia

* O atributo representa um conceito importante no seu dominio?

Atributo de peso em uma aplicativo de Academia
Atributo de Localizacao(contendo latitude e longitude) em um aplicativo de geolocalizacao

