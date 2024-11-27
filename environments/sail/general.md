Debugger setup for sail is one of the easiest, since guys from Laravel setup almost everything for you.

All you need to do is add one variable in your project's `.env` file

`SAIL_XDEBUG_MODE=develop,debug`

After that you need to restart sail.


After that you click on "Start listening for PHP debug connections in PhpStorm"
![Start listening fordebug connections](image.png)

And then you can enable "Break at first line in PHP script"
![Break at first line in PHP script](image-1.png)

Last thing you need is Xdebug helper browser extension.
Extensions links:
[Firefox](https://addons.mozilla.org/en-GB/firefox/addon/xdebug-helper-for-firefox/)
[Chrome](https://chromewebstore.google.com/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc)
[Edge](https://microsoftedge.microsoft.com/addons/detail/xdebug-helper/ggnngifabofaddiejjeagbaebkejomen)

When you have it installed, you can enable debugger - like displayed in the image below.
![Enable debugger](image-3.png)


Now you can refresh your Laravel app. You should get a popup for incomming xdebug connection from PHPstorm.
![PhpStorm popup for incoming xdebug connection](image-2.png)

Click Accept.

Program is stopped at the first line of Laravel application. Debugger is working 🚀.

But we are not done yet!

Stop the current debugging session, by clicking red stop button on bottom left in debugger toolbar.

By clicking accpet php server was automatically configured in phpstorm.

Since we are using Sail which is based on docker we need to setup path file mapping as well.
So debugger will now which files map together between our local machine (where we run Phpstorm) and our Docker containers.

So open settings (Ctrl + Alt + S) or File > Settings

Go under PHP > Servers

You will see the server with name "0.0.0.0". This server was automatically created when we clicked accept on incoming xdebug connection.
Now we need to setup path mapping, by adding `/var/www/html` in to first available cell in table, like shown in image below.
This tells phpstorm which local folder maps to which Docker container's folder.

![Setup path mapping in phpstorm server](image-4.png)


Now your debugger should be setup successfully. You can disable "stop on first line" and add breakpoints wherever you want in your project and the execution should be stopped at that line.