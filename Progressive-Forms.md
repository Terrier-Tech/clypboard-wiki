** Introduction 

Progressive Forms offers a way to present feedback to the user as a long running task is executing, allowing them to see the progress and potentially cancel the operation. It is based on the same technology used to execute scripts, but made more generic so that it can be used on most forms in the application with minimal modification.

** Controller 

In order to implement a progressive form, you must include ActionController::Live in your controller. Then, in the form action, execute all of your business logic inside the _execute_ method: 

```ruby
class FooController < ServerController 
  include ActionController::Live

  def exec
    progressive do |p|
      # some operations
      p.update 0.34, "Doing something"
      # some more operations
  end
end
```

Inside the _progressive_ block, you can call _update_ on the object passed into the block in order to send update messages to the user. The arguments are 1) the fraction of the total operation completed, and 2) a message to display to the user. 

For example, if you were looping through some locations and performing updates, your action might look like this: 

```ruby
def exec
  progressive do |p|
    locs = Locations.where(zip: '55124')
    count = locs.count
    loc.each_with_index do |loc, i|
      p.update i.to_f/count, "Updating location #{loc.display_number}"
      loc.update! state: 'MN'
    end
  end
end
```