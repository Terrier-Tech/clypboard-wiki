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

The corresponding model for the controller has dependencies as well.  In the model the field 'display_name' and function 'self.icon' must be defined.  If your model doesn't use a display_name field an attribute accessor may be used as an alternative.

```ruby
class Foo < MODEL_BASE
  def self.icon
    'foo'
  end

  @display_name = 'foo'
  attr_accessor :display_name
end
```

Then add the appropriate REST routes to routes.rb:

```ruby
resources :foos do 
end
```

Note that the controller and route names should be plural.

## Index/Show Views

As can be seen in the example above, CrudController does *not* implement an index action by default. This is because basically every one would have to be different, so I didn't see much point. 

It does, however, implement a *show* action (e.g. "/foos/#{foo.id}"), which will load the record based on id and render show.html.haml (or a JSON representation of the object if .json is applied to the id in the path).

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

Alternatively, you can provide a form that doesn't have *remote: true* and it will be rendered using separate pages. In this case, the form will be shown on its own page with either the _new_toolbar or _edit_toolbar partials used as the toolbar.

Finally, if you'd like to present different content for the new and edit forms, you can provide implementations for new.html.haml and edit.html.haml directly. 

## Lifecycle

While the default behavior of CrudController is sufficient for simple usage, it is often necessary to customize it for specific use cases. This can be done by implementing one or more of the following methods in the controller:

```ruby
def before_form(record)
  # gets called before the new and edit forms are shown
end

def on_create_redirect(record)
  # return a path to redirect to after the record is successfully created
  # defaults to reloading the current page
end

def create_error_template(record)
  # returns a template name to use if the creation fails
  # defaults to "#{model_name_plural}/form"
end

def after_create(record)
  # gets called after the record is successfully created
end

def on_update_redirect(record)
  # return a path to redirect to after the record is successfully updated
  # defaults to reloading the current page
end

def update_error_template(record)
  # returns a template name to use if the update fails
  # defaults to "#{model_name_plural}/form"
end

def after_update(record)
  # gets called after the record is successfully updated
end
```

If you need to provide some more specific behavior on a particular action, like overriding the default toolbar partial or changing the page title, you can override the action and call super:

```ruby
def edit
  @toolbar = 'foos/custom_toolbar'
  @title = 'Edit a Foo'
  super
end
```

## Soft Delete

By default, using the destroy action will actually delete the record: 

```haml
%a{href: "/foos/#{foo.id}", data: {method: 'DELETE'}} Delete Foo
```

If, instead, you'd like the record to be soft-deleted (_state set to 2), implement *should_soft_destroy* on the *model class*:

```ruby
def should_soft_destroy
  true
end
```

Because deletion can happen from various places in the application, the redirect behavior is not coded into the controller, like it is for create and updated. Instead, you can pass either a *redirect* or *reload* parameter to the action. This will a path to redirect after the destroy, or instruct the controller to reload the current page, respectively. The default behavior is to redirect to the *index* action.



