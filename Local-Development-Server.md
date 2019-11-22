Pow is no longer supported, so Clypboard uses puma-dev for serving the application in a local development environment: 

https://github.com/puma/puma-dev

Installation:

```
brew install puma/puma/puma-dev
sudo puma-dev -setup
puma-dev -install
```

Clypboard assumes that the application is hosted at `clypboard-server.test`. This means that your root directory should be named 'clypboard-server'. If, for example, you have your source code in ~/Clypboard/clypboard-server, here's how you activate it as a puma-dev app:

```
cd ~/.puma-dev
ln -s ~/Clypboard/clypboard-server
```

When you make configuration changes to the application, make sure to call `restart_tail` in order for them to take effect. 
