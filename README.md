# Playlist Tutorial-DBM
http://bit.ly/DiscordBotMakerPlaylist

Donate: https://goo.gl/EP28mh

# Downloads necessários
+ Node.Js: https://nodejs.org/en/
+ MODS: https://github.com/dbm-network/mods

# Script
Script usado no comando Prefixo
+ Run Script
```javascript
const filePath = require("path").join("data","serverPrefixes.json");	

let newTag = tempVars("tag");

if(Array.isArray(this.prefixes)){ 
  console.log("Versão antiga serverPrefixes.json detectada. Por favor, remova serverPrefixes.json e reinicie seu bot, caso contrário, definir novos prefixos não funcionará!")
  newTag = tag;
}

server.tag = newTag; 
Bot.prefixes[server.id] = newTag	   
require("fs").writeFileSync(filePath, JSON.stringify(Bot.prefixes));
this.callNextAction(cache);
```

Script usado no Evento Carregar Server Prefixo
+ Action 1 - Run Script
```javascript
// Isso deve ser executado a penas um vez!!
//------------

if(!globalVars("server_prefixes_loaded")){ 
  this.callNextAction(cache);
  this.storeValue(true, 3,"server_prefixes_loaded", cache);
}
```
+ Action 2 - Run Script
```javascript
Bot.prefixes = {};
console.log('(Server Prefixes) Iniciando prefixos de servidor....');
Bot.checkCommand = function(msg) {
	const fs = require("fs");
	const path = require("path");
   
    if (!msg.guild) return;

	try {
		const defaultTag = Files.data.settings.tag;
		const separator = Files.data.settings.separator || '\\s+';
	
		let content = msg.content;
		let guild = msg.guild;
	
		content = content.split(new RegExp(separator))[0];
	
		const filePath = path.join("data","serverPrefixes.json");		
		if(fs.existsSync(filePath)){
			let saved_prefixes = fs.readFileSync(filePath, "utf8");
			try {
				this.prefixes = JSON.parse(saved_prefixes);		
			} catch(err){}			
		}else{
			this.prefixes[guild.id] = defaultTag;
			fs.writeFileSync(filePath, JSON.stringify(this.prefixes));
		}

		let tag = this.prefixes[guild.id] || defaultTag;

                if(Array.isArray(this.prefixes)){ 
                  console.log("Old version serverPrefixes.json detected.  Please remove serverPrefixes.json and restart your bot otherwise setting new prefixes will not work!")
                  tag = defaultTag;
                }

		if(tag){	
			if(guild) guild.tag = tag;	

			if(content.startsWith(tag)) {				
				let command = content.substring(tag.length);
				if(command) {       
					if(!Bot._caseSensitive) {
						command = command.toLowerCase();
					}
					const cmd = Bot.$cmds[command];
					if(cmd) {					
						Actions.preformActions(msg, cmd);
					}
				}				
			}
		}			
	} catch (e) {
		console.error(e);
	}
   
};
console.log('(Server Prefixes) Carregado Funcao de Substituicao: CheckCommand');
```

Separador de CMD Categoria
```sh
<b><center><font size=3><font color="#ffffff">▷ VOICE VIP
```
