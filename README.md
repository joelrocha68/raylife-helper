# Raylife Starter

Welcome to board, Liferer!
> This doc was wrote to help you to configure your machine to start coding in our project!


We suppose that you're on a Liferay Machine with the liferay-portal's repository and other pre-configurations. 

## Summary
TBD

## Updating the local repository
> Check before if your `origin` repository is a fork project from Liferay and if your `upstream` is the original Liferay's project using `g remote -v`!

### Update everything after get the Raylife branch

Update your fork with last commit from original project:
```bash
$ g co master && g pull upstream master && g push -f
```

### Add the solutions' repository

Choose a alias for the solutions' repository by `<NAME>`. eg: `solution` or `pt-liferay-solution`
```bash
$ g remote add <NAME> git@github.com:pt-liferay-solutions/liferay-portal.git
```

Update the Raylife´s branch
```bash
$ git fetch solution raylife
```
> Remember: `solution` is the alias choosen in the step before, notice to change if you choose other. 


Create a new local branch to get the branch and choose your name by `<BRANCH>`. eg: `raylife`
```bash
g co -b <BRANCH>
```
> Notice that choose `raylife` as name of your new branch does not represent the real `solutions/raylife` that we fetched before. 

Update your new branch with the real Raylife's branch
```bash
$ g rebase solution/raylife
```

Compare your log with the commit list in the [Github Page](https://github.com/pt-liferay-solutions/liferay-portal/commits/raylife)
```bash
$ g log
```

If isn't equal, try this
```bash
$ g reset solutions/raylife --hard
```

PUF! Finally you have the most update Raylife's code! Let's continue!

## Build 'em up!

After update so many thing we need to build the Liferay Portal!
```bash
$ cdt && ant clean && ant all
```

## (Optional) First settings on Liferay Portal
> If you want change to Hypersonic to MySQL or forgot the credentials to access, you need do this below ;)

Delete the cache settings
```bash
$ cd ~/dev/bundles/master
$ rm -rf data elasticsearch7 osgi/state
```

Delete the settings file
```bash
$ rm portal-setup-wizard.properties
```

Up the project in Tomcat
```bash
$ catalina run
```

Up the MySQL on Docker
```bash
$ d-mysql8
```

> Yes, strange command, right? I'm using custom [alias](#alias)!

Wait for it. After open (alone) the `localhost:8080` in browser, you need to fill the mail field with `test@liferay.com` and password with `test`. 

In database topic, click on _(change)_ e choose MySQL.
Replace the Username and Password fields with the you are using to up your database.

After fill everything correctly, click on _Finish_. At last, after load, restart the `catalina` using Ctrl + C to down, and running the command above again, to up it.

All right!!! Sign in with `test@liferay.com` and `test` as password aaaandd WELCOME to Liferay Portal.

## Up the Remote Web Component

We have a remote web component to use in the Liferay Portal
```bash
$ cdt && cd modules/apps/site/site-insurance-site-initializer/remote-web-components/raylife-d2c-web/
```

Install the dependencies
```bash
$ yarn
```

Build the project to Liferay
```bash
$ yarn build:liferay
```

Up the server
```bash
$ yarn serve:liferay
```

If you open on `localhost:5000`, you will see the project, but we need put it inside the Liferay Portal.

For us, it's important the path? `localhost:5000/liferay/js`.

## Setting Widget Tool Module

After Sign in on Liferay Portal, go to *Global Menu (Grid icon) > Control Panel > System Settings* and in _Content and Data_ topic notice that there aren't a _Widget Tool_. We need deploy it to Liferay Portal
```bash
$ cdt && cd modules/apps/remote-web-component/remote-web-component-admin-web && gw clean deploy
```

Refrash the page and _look the magic!_

We need to add a configure entry to our remote web component started in the section above, so click on _Add_.

The next step it's fill the follow fields:
- Web Component Name (eg. d2c-web)
- Custom Element Name (with same Web Component Name) 
- Web Component URL(s) (with files present on `localhost:5000/liferay/js` in the remote web component)
	- For more than one, click on _+_ to open another field to put the other path
- Portlet Display Category (with same Web Component Name)

After click on _Save_ you are able to use the Remote Web Component whenever it's up at `localhost:5000`.

## Create a Raylife with Site Inicializer and use the Remote Web Component
- Go to *Global Menu (Grid icon) > Control Panel > Sites*.
- Click on _+_ blue icon.
- Select Raylife.
- The name field ALWAYS be `Raylife`.

_Tcharam!_

On URL, at the final, add `get-a-quote`. In this page, edit, clicking on _pencil_ icon at the top bar e search in _Widgets_ for our new component. Drag and Drop it! Have fun!


## Known issues

### Raylife's Product Categories doesn't appear on Home page or there more than normal (more than 6)
TBD

### Remote Web Component appear as `html` tag.
TBD

## Miscellaneous

### Alias
> Because programmers are lazy ;)

> These custom settings need to be putting inside `~/.bashrc`.

To use `catalina` for up and down our Liferay Portal
```
alias catalina="~/dev/bundles/master/tomcat-*/bin/catalina.sh"
```

To use, we need
```bash
$ catalina run
$ catalina stop
```

To up a MySQL in background inside Docker
```
alias d-mysql="d run -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -e MYSQL_DATABASE=lportal -e MYSQL_PASSWORD=<PASSWORD> -e MYSQL_USER=<USER> -d --restart=always -p 3306:3306 mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci"
```

> Remember to change `<PASSWORD>` and `<USER>` with the credentials that you configure the Liferay Portal initialy.

To use, we need
```bash
$ d-mysql8
```