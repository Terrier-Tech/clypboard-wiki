The CrudController is a useful base class for controllers that provide CRUD operations on models. This should only be used for server-side components - all app CRUD should still be done through tinysync.

To use CrudController, make a subclass and assign a model:

```ruby
class FoosController < CrudController 

  model Foo

end
```

