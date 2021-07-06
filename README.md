# laravel-form

## Install

### Step 1: Install via Composer

``` bash
$ composer require webup/laravel-form
```

### Step 2: Add the Service Provider

Add this line to config/app.php:

``` php
'providers' => [
    //...
    Webup\LaravelForm\FormServiceProvider::class
]
```

### Step 3: Use Facade (optional)

For shorter code, you can use the facade by adding this line to config/app.php:

``` php
'aliases' => [
    //...
    'Form'      => Webup\LaravelForm\Facades\Form::class,
]
```

You can now use Laravel Form directly into your views (check some examples bellow)

### Step 4: Publish config

You can publish config and override it in config/form.php:

```
 php artisan vendor:publish
```

## Using

### Methods

These methods can be used with any type of elements:

* label($label = null, $escape = true)
* value($value = null)
* placeholder($placeholder = null)
* name($name = null)
* required($showStar = true)
* errors($errors = [])
* attr(array $attr = [])
* wrapperAttr(array $attr = [])
* wrapperClass($wrapperClass) **deprecated** use wrapperAttr(['class' => 'myclass'])

### Generated HTML


``` php
{!! Form::create('text', 'name')
    ->label('Name')
    ->value('Barney')
    ->required()
    ->attr(['maxlenght' => '50'])
    ->wrapperAttr(['class' => 'f-custom-class']) !!}
```

Without errors:

``` html
<div class="f-group f-custom-class">
    <label for="name">Name <i class="f-required">*</i></label>    
    <input type="text" id="name" name="name" value="Barney" maxlength="50">
</div>
```

With errors (retrieve from Laravel validation):
``` html
<div class="f-group f-custom-class f-error">
    <label for="name">Name <i class="f-required">*</i></label>    
    <input type="text" id="name" name="name" value="Barney" maxlength="50">
    <ul class="f-error-message">
        <li>Name is required</li>
    </ul>
</div>
```

You can override default CSS class in config/form.php.  

**Important**: Laravel Form can handle HTML generation and client side  validation only. You need to manage server side validation on your own.

### Elements
#### input

``` php
{!! Form::create('email', 'name')
    ->label('Email')
    ->value('homer.simpson@example.com')
    ->placeholder('example@adresse.com')
    ->required()
    ->wrapperAttr(['class' => 'custom-class']) !!}
```

#### textarea

``` php
{!! Form::create('textarea', 'name')
    ->label('Name')
    ->value('Nullam id dolor id nibh ultricies vehicula ut id elit.') !!}
```

#### radio

``` php
{!! Form::create('radio', 'gender')->label('Gender')
    ->addRadio(1, 'Male', 'male')
    ->addRadio(0, 'Female', 'female')
    ->wrapperAttr(['class' => 'custom-class'])
    ->value(0) !!}
```

Specific methods :

* addRadio($value, $label, $id, $attr = [])

#### select

``` php
{!! Form::create('select', 'fruits')
    ->label("Fruits")
    ->placeholder("What's your favorite?")
    ->addOptions(['apple' => 'Apple', 'strawberry' => 'Strawberry', 'melon' => 'Melon'])
    ->value('apple') !!}
```

Specific methods :

* addOptions(array $options)

#### checkbox

``` php
{!! Form::create('checkbox', 'cgu')
    ->label("I accept the general terms and conditions")
    ->value(true) !!}
```

You can use `->attr(['value' => '1'])` to change the value of the checkbox

## AntiSpam feature

### Honeypot

``` php
{!! Form::honeypot("unicorn_mail") !!}
```
Will create an input `text` with `name='unicorn_mail'` within a hidden div (by javascript)

#### Validation

``` php
$request->validate([
    [...]
    'unicorn_mail' => 'honeypot',
]);
```

### TimeTrap

``` php
{!! Form::timetrap("unicorn_time") !!}
```
Will create an input `text` with `name='unicorn_time'` and `value="{encryptedTimestamp}"` within a hidden div (by javascript)

#### Validation

``` php
$request->validate([
    [...]
    'unicorn_time' => 'timetrap:2',
]);
```
In this example, timetrap time is set to `2` seconds. If no value is set, config `form.antiSpam.minFormSubmitTime` is taken. Finally if config `form.antiSpam.minFormSubmitTime` is not set, default value is `3` seconds.

## Styling

Bellow, you will find default styles that can work with Laravel Form.

``` css

.f-group {
  margin-bottom: 1rem;
}

.f-group label {
  margin-bottom: .5rem;    
}

.f-required {
  color: #c0392b;
  font-weight: bold;
}

.f-error input {
  border: 1px solid #c0392b;
}

.f-error-message {
  margin-top: 4px;
  color: #c0392b;
}

```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) and [CONDUCT](CONDUCT.md) for details.

## Credits

Developed by [Agence Webup](https://github.com/agence-webup)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
