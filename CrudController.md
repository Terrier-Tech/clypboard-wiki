The CrudController is a useful base class for controllers that provide CRUD operations on models. This should only be used for server-side components - all app CRUD should still be done through tinysync.

To use CrudController, make a subclass and assign a model:

```ruby
class FoosController < CrudController 

  model Foo

  def index
    @foos = Foos.where(_state: 0)
  end

end
```

Then add the appropriate REST routes to routes.rb:

```ruby
resources :foos do 
end
```

Note that the controller and route names should be plural.

## New/Edit Views

By default, CrudController will handle all CRUD operations as-is, so there's no need to implement those actions explicitly. Simply provide the appropriate view(s) and the controller will provide reasonable behavior for the actions. This includes performing validation and saving using the user changes method (.save_by_user?).

The simplest way to implement the views would be to provide a _form.html.haml partial:

```haml
= form_for @foo, remote: true do |f|
    = render partial: 'shared/validation_messages', object: @foo
    = f.label :name
    = f.text_field :name
```

Because this form is meant to be shown in a modal, it needs to have *remote: true* specified. Also note that CrudController automatically creates an instance variable named after the model class (@foo, in this case) for each action.

This form can be shown using simple links with the *modal* class: 

```haml
%a.modal{href: "/foos/new"} New Foo
%a.modal{href: "/foos/#{foo.id}/edit"} Edit Foo
```

