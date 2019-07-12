---
layout: post
title:  "What Steam-Game should I play? Let JavaScript decide!"
date:   2018-07-10 11:50:00 +0200
categories: Tutorial
---
Having over 800 games on steam was a huge mess for me. I needed to decide which game I should play. For this I used the WhatShouldISteam-Page, but we wouldn’t be programmers if we didn’t achieve this stuff ourselves. So let’s find a game to play!

To use the [SteamWebAPI](https://developer.valvesoftware.com/wiki/Steam_Web_API) we will need two information:

- [Your Steam-Api-Key](https://steamcommunity.com/dev/apikey)
- [Account-Id (64)](https://steamid.io/)
After we collected these two information we can start with our logic of finding our desired game:

```js
let currentGame = {};
let ownedGames = [];
 
let steamID = "";
let apiKey = "";
 
//Simple JSON-XMLHttpRequest-Function to work with the API-Service
let getJSON = function(url,callback) {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', url, true);
    xhr.responseType = 'json';
    xhr.onload = function() {
      var status = xhr.status;
      if (status === 200) {
        callback(null, xhr.response);
      } else {
        callback(status, xhr.response);
      }
    };
    xhr.send();
}
 
//Get all of our ownedGames
function getOwnedGames(apiKey, steamID) {
    //Parse our steamID and apiKey directly into our url
    var url = `http://api.steampowered.com/IPlayerService/GetOwnedGames/v0001/?key=${apiKey}&steamid=${steamID}&include_appinfo=1`;
    getJSON(url, function(err, data) {
        //Hopefully will never trigger
        if (err !== null) {
            console.error('An error occured: ' + err);
        } else {
            
            ownedGames = data.response.games;
            getRandomGame();
        }
    });
}
 
//Get a single random game
function getRandomGame(){
 
    var randomGame = Math.floor(Math.random() * (ownedGames.length + 1));
    //now you can do everything you want with it: open the game, show it on screen etc
    
    currentGame = ownedGames[randomGame];
    
    /**
     * 
        {
          appid: 366260
          has_community_visible_stats: true
          img_icon_url: "3335e8070277902ea30bc33f011a0b30b2183b3e"
          img_logo_url:"cdb1fd917b7f3261c3dfdaa220dcef002abfd5a6"
          name:"METAL SLUG 2"
          playtime_forever:0
        }
    */
    console.log(currentGame.name);
    
}
 
getOwnedGames(apiKey, steamID);
```

I have added some comments to the code to break the functionality down to you. Now that you know how to find a random game of your own library you might want to expand the functionality with an HTML-UI and actually build a page to get some more information about the game and a “Start”-Button.

Other possible Features could be:

- Find a random, not owned yet, game by name
- Find a random, not owned yet, game by category
- Find a random, owned, game by name
- Find a random, owned, game by category
- Get more information about that game like pictures, descriptions and rating
There are many possibilities.