---
layout: post
published: true
title: Xenforo 2.x Addon Notes
date: '2020-11-05'
---
## Creating the Addon

The first step in creating a new Xenforo Addon is creating the addon within the Xenforo dev environment with the following command: `php cmd.php xf-addon:create`. 

Last night I set up a development environment for Xenforo using (XAMPP)[https://www.apachefriends.org/index.html]. I was able to install Xenforo and create an addon without a problem on Windows. 

However, when I tried to do this on Mac I got the following error:

```
An exception occurred: [XF\Db\Exception] No such file or directory in src/XF/Db/Mysqli/Adapter.php on line 173
#0 src/XF/Db/Mysqli/Adapter.php(28): XF\Db\Mysqli\Adapter->makeConnection(Array)
#1 src/XF/Db/AbstractAdapter.php(60): XF\Db\Mysqli\Adapter->getConnection()
#2 src/XF/Db/Mysqli/Adapter.php(113): XF\Db\AbstractAdapter->connect()
#3 src/XF/Db/AbstractAdapter.php(516): XF\Db\Mysqli\Adapter->escapeString('addOnsComposer')
#4 src/XF/Db/AbstractAdapter.php(494): XF\Db\AbstractAdapter->quote('addOnsComposer')
#5 src/XF/DataRegistry.php(138): XF\Db\AbstractAdapter->quote(Array)
#6 src/XF/DataRegistry.php(83): XF\DataRegistry->readFromDb(Array, Array)
#7 src/XF/DataRegistry.php(226): XF\DataRegistry->get(Array)
#8 src/XF/App.php(1650): XF\DataRegistry->offsetGet('addOnsComposer')
#9 src/XF/Container.php(28): XF\App->XF\{closure}(Object(XF\Container))
#10 src/XF/App.php(2164): XF\Container->offsetGet('addon.composer')
#11 src/XF/App.php(1749): XF\App->setupAddOnComposerAutoload()
#12 src/XF/Cli/App.php(25): XF\App->setup()
#13 src/XF.php(364): XF\Cli\App->setup(Array)
#14 src/XF/Cli/Runner.php(50): XF::setupApp('XF\\Cli\\App')
#15 cmd.php(15): XF\Cli\Runner->run()
#16 {main}
```

Turns out that the problem was in my `config.php` file where I had `host` configured as `localhost`:

```php
$config['db']['host'] = 'localhost';
```

On a hunch, I figured I would try the following:

```php
$config['db']['host'] = '127.0.0.1';
```

And it worked! Now I'm able to create new addons on my Mac without a problem.