
app/
    assets/
        javascripts/
            app/  # all app coffeescript files
            app.js  # list of required javascript files that will be compiled together for the app page
            server.js # compiled server JavaScript (also used in app)
            vendor.js # compiled JavaScripts from /vendor, this is a separate bundle because it changes much less frequently than server.js
        stylesheets/
            app/  # all app scss files
            portal/ # all portal scss files
            app.css  # compiled app styles
            portal.css  # compiled portal styles
            server.css  # compiled server styles (also used in app)
    card_processors/  # Ruby classes that interface with payment providers (Stripe and Heartland)
    controllers/
        api/  # controllers for the TinySync API, which inherit from Api::ApiController
        app/  # all app controllers, which should inherit from App::AppController
        portal/  # all controllers for the portal
    services/  # useful Ruby classes that don't belong anywhere else
    views/
        app/  # all app views, in subdirectories based on the controller name
        portal/  # all views for the portal

**NOTE:** All app actions and views (except for those that won't be used on the device, like the login screen)
should _not_ contain any Ruby code. All user interface logic must be done on the client side.