**Profesión del usuario** (2 puntos).  

Crear un campo de texto para almacenar la profesión, que será escrita por el usuario en caso de  que no se encuentre en la lista que tenemos. Por tanto, ahora *la profesión será obligatoria*. Si está en el  select, la elige y si no está, la escribe en el campo a crear. Cada una será requerida si no está presente  la otra. Desarrollar las pruebas, interfaz y back-end para que funcione.

# Resumen

Para que la profesión sea requerida se debn modificar las siguientes clases:
- *CreateUserRequest:* Esta clase guarda las validaciones y las reglas , se debe modificar la validacion de profession_id.

```php
'profession_id' => [  
    'required',  
    Rule::exists('professions', 'id')  
                    ->whereNull('deleted_at')  
],
```

- *CreateUserTest:* En esta clase es importante tomar en cuenta que la propiedad `$defaultData` se valida a travéx de los helpers que lo utiliza `getValiData()`, por eso es importante crear un factory en cada ya que ahora el campo profession es requerido.

 ```php
 protected $defaultData = [  
    'first_name' => 'Pepe',  
    'last_name' => 'Pérez',  
    'email' => 'pepe@mail.es',  
    'password' => '12345678',  
    'profession_id' => '1',  //agregado
    'bio' => 'Programador de Laravel y Vue.js',  
    'twitter' => 'https://twitter.com/pepe',  
    'role' => 'user',  
    'state' => 'active',  
];


/** @test */  
function the_twitter_field_is_optional()  
{  
$profession = factory(Profession::class) ->create();  
    $this->withoutExceptionHandling();  
    $this->post('usuarios', $this->getValidData([  
        'twitter' => null,  
  'profession_id'=>$profession->id  
    ]))->assertRedirect('usuarios');  
  
    $this->assertCredentials([  
        'first_name' => 'Pepe',  
        'email' => 'pepe@mail.es',  
        'password' => '12345678',  
  
    ]);  
  
    $this->assertDatabaseHas('user_profiles', [  
        'bio' => 'Programador de Laravel y Vue.js',  
        'twitter' => null,  
        'user_id' => User::findByEmail('pepe@mail.es')->id,  
 'profession_id'=>$profession->id,  
    ]);  
}

/** @test */  
function the_role_field_is_optional()  
{  
 $profession = factory(Profession::class) ->create();  
   
 $this->post('usuarios', $this->getValidData([  
  'role' => null,  
 ]))->assertRedirect('usuarios');  
   
 $this->assertDatabaseHas('users', [  
  'email' => 'pepe@mail.es',  
  'role' => 'user',  
 ]);  
}
 ```

- *UpdateUserTest:* esta clase mesomenos hara lo mismo que la anterior se le dabe pasar profession_id.

- *CreateUserProfile:* En esta clase se debe quitar la profesion como null.

```php
$table->unsignedInteger('profession_id');  
    //->nullable();
```

- *UserProfileFactory:*  Se debe crear la profesion ya que en la base de datos no existe.

```php
$factory->define(App\UserProfile::class, function (Faker $faker) {  
    return [  
        'bio' => $faker->paragraph,  
    'profession_id'=>factory(\App\User::class)  
    ];  
});
```