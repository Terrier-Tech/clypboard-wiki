## Introduction 

Progressive Forms offers a way to present feedback to the user as a long running task is executing, allowing them to see the progress and potentially cancel the operation. It is based on the same technology used to execute scripts, but made more generic so that it can be used on most forms in the application with minimal modification.

## Controller 

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

## Response 

Once the code inside the _progressive_ block has executed, the controller will render the view for the action and send it back to the client. Because progressive forms use streaming JSON to send messages from the server to client, the forms **must** be remote, so the views must be .js.erb files. This is the same response format used by regular remote forms, so it requires no view modification to make an existing form to progressive.

An example view would be something like:

```javascript
// exec.js.arb
$('.modal-content').html('<h4 class="text-center">Successfully Executed Progressive Form</h4>')
$('.modal-actions').remove()
```



