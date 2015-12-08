![NPM](https://nodei.co/npm/socket-anti-spam.png?downloads=true&downloadRank=true&stars=true)

[ ![Image](https://david-dm.org/michaeldegroot/socket-anti-spam.svg "deps") ](https://david-dm.org/michaeldegroot/socket-anti-spam "david-dm")
[ ![Image](https://travis-ci.org/michaeldegroot/socket-anti-spam.svg?branch=master "testing") ](https://travis-ci.org/michaeldegroot/socket-anti-spam "travis-ci")
[![Coverage Status](https://coveralls.io/repos/michaeldegroot/socket-anti-spam/badge.svg?branch=master&service=github)](https://coveralls.io/github/michaeldegroot/socket-anti-spam?branch=master)
![NPM](https://img.shields.io/badge/Node-%3E%3D0.10-green.svg)
![](https://img.shields.io/npm/dt/socket-anti-spam.svg)
![](https://img.shields.io/npm/l/express.svg)

# What it does

##### Tired of users spamming your socket.io emits? this module prevents this. #####

## Changelog

### 5/8/14
 - Changed the readme (added a last step that I forgot to mention but it was detailed in the example 'How does it look'
 
### 5/7/14
 - Can now temp ban after x amount of kicks within x defined of time.
 - Client detection based on ip now instead of socket.id (This is how we keep track of amount kicks and ban accordingly)
 
### 12/5/15
 - Removed array.prototype.contains as it is not used anywhere


# How does it look?

![socketspam.gif](https://bitbucket.org/repo/kR4677/images/1013607973-socketspam.gif)


	// Everyone has this line already when using socket-anti-spam
	var io = require('socket.io').listen(8080,{ log: false });

	// Needed for socket-anti-spam to work correctly
	var antiSpam = require('./antispam');
	var antiSpam = new antiSpam({
		spamCheckInterval: 3000,
		spamMinusPointsPerInterval: 3,
		spamMaxPointsBeforeKick: 9,
		spamEnableTempBan: true,
		spamKicksBeforeTempBan: 3,
		spamTempBanInMinutes: 10,
		removeKickCountAfter: 1,
		debug: false
	});

	 // Everyone has this line already when using socket-anti-spam
	io.sockets.on('connection', function (socket) {
		antiSpam.onConnect(socket); // Needed for socket-anti-spam to work correctly
	});

## And now you are safe from people repeatedly spamming your sockets (based on ip)!


#  How do I use it?

## 1. Start by installing the package:
    npm install socket-anti-spam

## 2. Put this in your server side file AFTER you have defined socket.io variable:
	var antiSpam = require('socket-anti-spam');
	var antiSpam = new antiSpam({
		spamCheckInterval: 3000, // define in how much miliseconds the antispam script gives a minus spamscore point
		spamMinusPointsPerInterval: 3, // how many minus spamscore points after x miliseconds?
		spamMaxPointsBeforeKick: 9, // needed points before kick
		spamEnableTempBan: true, // Enable the temp ban system (temp ban users after x amount of kicks within x amount of time)
		spamKicksBeforeTempBan: 3, // This many kicks needed for a temp ban
		spamTempBanInMinutes: 10, // This many minutes temp ban will be active
		removeKickCountAfter: 1, // This many minutes until the kick counter is decreasing with 1 for the user
		debug: false // debug? not needed
	});
	
## 3. Put this inbetween the socket.on connection @ server side
	io.sockets.on('connection', function (socket) {
		antiSpam.onConnect(socket);
	});
	
Now all sockets will be individually checked if they spam your socket.emits and if they do they will be disconnected, after to many repeated offenses they will be temp banned (ip based).

# Contact
    You can contact me at specamps@gmail.com