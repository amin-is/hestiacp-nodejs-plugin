# HestiaCP add multiple NodeJS apps using QuickApp Installer.

You can add multiple websites to your HestiaCP using diferent ports to each one.

When you create create the app with the installer it automatically create:
* **/home/%USER%/%DOMAIN%/private/nodeapp** directory
* Config for **nginx** to use the selected port
* **ecosystem.config.js** with the necessary command to connect **pm2** and run your app Ex. `npm run start`
* **.nvmrc** file with node version if you use NVM

## How to install

1. Install Node: (Using one of these options)
   *  [NodeJS](https://github.com/nodesource/distributions) 

	```bash
 	sudo curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash - && sudo apt install nodejs -y
	node --version && npm --version
	```
3. Install [PM2](https://pm2.keymetrics.io/)
	```bash
	npm install pm2@latest -g
	```

4. Clone this repository:
	```bash
	cd /home
	git clone https://github.com/amin-is/hestiacp-nodejs-plugin.git
	cd hestiacp-nodejs-plugin
	```

5. Use **install.sh**
	```bash
	sudo chmod 755 install.sh
	sudo ./install.sh
	```
 6. **Delete Script Folder**
	```bash
 	cd ..
	rm -rf hestiacp-nodejs-plugin
	```

7. 🚀 You are ready to install an App!!!

## How to use

1. Create new **user** (If you have one no need to create)
2. User needs bash access for app to work, go to **User edit** > **advanced options** > **SSH Access** > **bash**
3. **Add** new web (Ex. acme.com)
4. Go to **edit** this new web and go to **Quick Install App**
5. Select **NodeJS**
   * **Node Version**: If you manage node with nvm, it put a `.nvmrc file in root of nodeapp with selected version. If you installed node without nvm you can remove this file.

   * **Start Script**: It creates a `ecosystem.config.js` file in root of nodeapp with the script that you fill (it should be the one you have in your `package.json`) for PM2 can manage the app.

   * **Port**: You can manage multiple apps with different ports, put different port for each app you have (Ex. 3000).
   It creates `.env` file in root of nodeapp with the selected port, if your app don't use this `.env` file you can remove.

   * **PHP Version**: This is only for HestiaCP you can put any value (**NOT IMPORTANT**)
6. Go to Edit web > Advanced Options > Proxy Template > NodeJS
7. Upload your app with filemanager, clone with git... in `/home/<user>/<domain.com>/private/nodeapp`
8. 🎉 Congratulations you're done!!!

9. Save PM2 & Run Forever
	```bash
 	pm2 list
 	pm2 restart all
	sudo pm2 save
	sudo pm2 startup systemd
	sudo systemctl start pm2-root
	```

## FAQ

### How to change the port if i have a web running

First change proxy template to default, reconfigure the app using the QuickInstall and finally change the proxy template to NodeJS.

### I want to remove the domain

Remove it normally, open the filemanager and remove hestiacp_nodejs_config/web/<domain.com>.
