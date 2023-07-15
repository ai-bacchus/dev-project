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
