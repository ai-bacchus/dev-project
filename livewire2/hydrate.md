1. hydrate Model for better performance. 

```php
class UserComponent extends Component
{
    public $email;
    public $name;

    protected $user;
    protected $listeners = ['refreshComponent' => 'hydrateUser'];

    public function mount(User $user)
    {
        $this->hydrateUser($user);
    }
    
    public function hydrateUser(User $user)
    {
        $this->user = $user;
        $this->email = $user->email;
        $this->name = $user->name;
    }

    public function render()
    {
        return view('livewire.user-component');
    }
}

```

2. Change value with '$set' instead of click
```php
<button wire:click="$set('showText', true)">Show</button>
```

3. True / False toogle easy .
```php
<button wire:click="$toggle('showText')">Show/Hide</button>
```
Or when client side only 

```php
<div x-data="{ open: false }">
    <button @click="open = true">Expand</button>
 
    <span x-show="open">
      Content...
    </span>
</div>
```

4. Validation Customization
```php
class ContactForm extends Component
{
    protected $validationAttributes = [
        'email' => 'email address'
    ];
 
    // ...

```
This is different with Laravel way. 


5. Loading
```php
// when it takes morethan 500ms.
<div wire:loading.delay.longer>...</div>
```

6. When Offline
```php
<div wire:offline>
    You are now offline.
</div>
<div wire:offline.class="bg-red-300"></div>
```


7. Pagination Theme.
```php
    use WithPagination;

    protected $paginationTheme = 'bootstrap';
    protected $paginationTheme = 'custom.pagination';
```

8. Delete Confirm modal
```php
<button
    onclick="confirm('Are you sure?') || event.stopImmediatePropagation()"
    wire:click="delete($post->id)"
>Delete</button>
```

9. Calling Server Method from inline Javascript
```js
function someFunction(event)
{
    event.stopImmediatePropagation();
    
    ...
    @this.call('delete')
}
```

10. Use Route Model Binding to fetch the model
```php
public function mount(User $user): void
{
    $this->fill($user);
}

// good 
<livewire:profile :user="auth()->user()->uuid" />
// bad
<livewire:profile :user="auth()->user()" /> 

```

11. No Error display on Production
```php
@production
  <script>
      Livewire.onError(function (message, response) {
          return false;
      });
  </script>
@endproduction

```


12. Use computed properties as like cache
```php
// bad
public function countries(): Collection
{
    return Country::select('name', 'code')
        ->orderBy('name')
        ->get();
}

//good
public function getCountriesProperty(): Collection
{
    return Country::select('name', 'code')
        ->orderBy('name')
        ->get();
}


// example

class ShowPost extends Component
{
    public $postId;
 
    public function getPostProperty()
    {
        return Post::find($this->postId);
    }
 
    public function deletePost()
    {
        $this->post->delete();
    }
}
<div>
    <h1>{{ $this->post->title }}</h1>
    ...
    <button wire:click="deletePost">Delete Post</button>
</div>

```

13. Entangle live data
Instead of using component like this (server-bound):
```php
<div>
    <input wire:model="count" type="number">
    <button wire:click="increment">+</button>
</div>
```
You can do it like this:
```php
<div x-data="{ count: $wire.entangle('count') }">
    <input x-model="count" type="number">
    <button @click="count++">+</button>
</div>

```


15. Use Form Request rules for validation
```php
// bad
public function rules(): array
{
    return [
        'field1' => ['required', 'string'],
        'field2' => ['required', 'integer'],
        'field3' => ['required', 'boolean'],
    ];
}

// good

public function rules(): array
{
    return (new MyFormRequest)->rules();
}


```

