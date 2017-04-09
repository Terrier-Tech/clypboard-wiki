Constants from Ruby that need to be used in Javascript are stored in the constants.js.erb file.  

Constants are specified as:
    `window.constants.paymentMethods = <%= Payment.payment_methods %>`

where `window.constants.paymentMethods` is how the object(s) will be referenced in JS and `Payment.payment_methods` is the Ruby definition of the object(s).

This file is a precompiled asset, so making changes to the Ruby definitions will not trigger the constants to recompile.  Unfortunately, at some point in the recent past, making changes to the constants.js.erb file itself also no longer triggers a recompile.

To force it to recompile, use the heavy handed method of deleting the cached files from the server before deploying the new changes.  SSH into the server and execute `rm -rf tmp/cache/*`.  This deletes all precompiled assets, such as `.scss`, so it will force everything to recompile, making the deployment take a very long time.  It is recommended to do this outside of regular business hours.