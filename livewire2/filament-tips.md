


## Form reactive with afterStateUpdated( )


```php 
class Form extends Component implements HasForms 
{
	use InteractsWithForms;

	public ?string $title ='';
	public ?string $slug = '';

	protected function getFormSchema(): array 
	{
		return [
			TextInput::make('title')
				->reactive()
				->afterStateUpdated( fn( $state, callable $set )=> $set('slug', Str::slug($state))),
			TextInput::make('slug')
		];
	}
}

```


## Dependant Fields/ Components 

```php

class Form extends Component implements HasForms 
{
	use InteractsWithForms;

	public ?string $title ='';
	public ?string $slug = '';

	protected function getFormSchema(): array 
	{
		return [
			Select::make('category_id')
			->label('Category')
			->options( Category::all()->pluck('title', 'id')->toArray())
			->reactive(),

			Select::make('post_id')
			->label('Post')
			->options( Post::all()->pluck('title', 'id')->toArray()),
		];
	}
}

```


## Field Lifecycle 

#### Hydration - afterStateHydrated
	- it runs when the 'fill()' method called. 

```php
use Closure;
use Filament\Forms\Components\TextInput;
 
TextInput::make('name')
    ->afterStateHydrated(function (TextInput $component, $state) {
        $component->state(ucwords($state));
    })

```	


#### Updates - afterStateUpdated() 
```php
use Closure;
use Filament\Forms\Components\TextInput;
use Illuminate\Support\Str;
 
TextInput::make('title')
    ->reactive()
    ->afterStateUpdated(function (Closure $set, $state) {
        $set('slug', Str::slug($state));
    })
TextInput::make('slug')

```


#### Dehydration 
-- it runs when  form's getState() method called .

```php
use Filament\Forms\Components\TextInput;
 
TextInput::make('name')->dehydrateStateUsing(fn ($state) => ucwords($state))

```
