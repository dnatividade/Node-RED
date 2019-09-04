# Node-RED
Node-RED instalation, configuration and projects examples

## Instalation and configuration guide

### Install Node-RED ###
#this procedure was tested in Debian 10 x64 enviroment

##Install node-RED
```
$ bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)
$ npm audit fix
```

#install password hash tool
```
$ npm install -g node-red-admin
```
###################################################################

##Creating a Self Signed Certificate

#source: http://www.steves-internet-guide.com/securing-node-red-ssl/
```
mkdir .node-red/nodecerts
cd .node-red/nodecerts/
```
#1.Create a private key
`$ openssl genrsa -out node-key.pem 2048`

#2. Create a certificate Request
`$ openssl req -new -sha256 -key node-key.pem -out node-csr.pem`

#3. Sign the Certificate with the Private key to create a self signed Certificate
`$ openssl x509 -req -in node-csr.pem -signkey node-key.pem -out node-cert.pem`
####################################################################


#configuration file: #####
#~/.node-red/settings.js
##########################

#uncomment:
```
	var fs=require("fs")
```

#in the AdminAuth section uncomment the https section and edit as shown in the screenshot below:
```
    https: {
        key: fs.readFileSync('/home/suporte/.node-red/nodecerts/node-key.pem'),
        cert: fs.readFileSync('/home/suporte/.node-red/nodecerts/node-cert.pem')
    },
```

#uncomment:
```
	requireHttps: true,
```
####################################################################


##Manangement page password
#create passwword hash (after enter with desired password)
`$ node-red-admin hash-pw`

#copy and paste created hash to section adminAuth, users, password:
```
	adminAuth: {
		type: "credentials",
		users: [
	{
			username: "admin",
			#put here your password hash
			password: "$2a$08$4I6mgDF9CHB1cUKCJoUBxurxWFYhkdyxb6n8UKtYh67XZrREnkl9W",
			permissions: "*"
		},
	{
			username: "suporte",
			#put here your password hash
			password: "$2a$08$4I6mgDF9CHB1cUKCJoUBxurxWFYhkdyxb6n8UKtYh67XZrREnkl9W",
			permissions: "*"
		}
		]
	},
```

##Dashboard password
#uncomment and configure httpNodeAuth line, as follows:
```
	httpNodeAuth: {user:"suporte",pass:"$2a$08$4I6mgDF9CHB1cUKCJoUBxurxWFYhkdyxb6n8UKtYh67XZrREnkl9W"},
  ```
####################################################################

##Configure static web pages directory
#uncomment httpStatic and configure path:
```
	httpStatic '/home/suporte/.node-red/node-red-static/',
```

##Create password to dashboard
#uncomment and configure httpStaticAuth line, as follows:
```
httpStaticAuth:{user:"suporte",pass:"$2a$08$4I6mgDF9CHB1cUKCJoUBxurxWFYhkdyxb6n8UKtYh67XZrREnkl9W"},
```

#create directory
```
$ mkdir node-red-static
```

#restart node-RED
```
$ sudo systemctl restart nodered.service
```
