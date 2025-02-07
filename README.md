[![npm version](https://img.shields.io/npm/v/sulla-hotfix.svg?color=green)](https://www.npmjs.com/package/sulla-hotfix)
[![Greenkeeper badge](https://badges.greenkeeper.io/danielcardeenas/sulla.svg)](https://greenkeeper.io/)

# sulla

> Sulla is a javascript library which provides a high-level API control to Whatsapp so it can be configured to automatize resposes or any data that goes trough Whatsapp effortlessly. 
>
> It is built using [puppeteer](https://github.com/GoogleChrome/puppeteer) and based on [this python wrapper](https://github.com/mukulhase/WebWhatsapp-Wrapper)


## Installation

```bash
> npm i --save sulla-hotfix
```

## Usage

```javascript
// import { create, Whatsapp } from 'sulla';
const sulla = require('sulla-hotfix');

sulla.create().then(client => start(client));

function start(client) {
  client.onMessage(message => {
    if (message.body === 'Hi') {
      client.sendText(message.from, '👋 Hello from sulla!');
    }
  });
}
```

###### After executing `create()` function, **sulla** will create an instance of whatsapp web. If you are not logged in, it will print a QR code in the [terminal](https://i.imgur.com/g8QvERI.png). Scan it with your phone and you are ready to go!
###### sulla will remember the session so there is no need to authenticate everytime.

### Functions list
| Function                          	| Description 	| Implemented 	|
|-----------------------------------	|-------------	|-------------	|
| Receive message                   	|             	| ✅           	|
| Send text                         	|             	| ✅           	|
| Get contacts                      	|             	| ✅           	|
| Get chats                         	|             	| ✅           	|
| Get groups                        	|             	| ✅           	|
| Get group members                 	|             	| ✅           	|
| Send contact                      	|             	| ✅           	|
| Get contact detail                	|             	| ✅           	|
| Send Images (image)    	            |             	| ✅             |
| Send media (audio, doc, video)    	|             	|             	 |
| Send stickers                     	|             	|             	 |
| Decrypt media (image, audio, doc) 	|             	| ✅            	|


## Decrypting Media

Here is a sample of how to decrypt media. This has been tested on images, videos, documents, audio. It does not work for voice notes right now, or I don't have the right codecs to play them.

```javascript
import { create, Whatsapp, decryptMedia} from 'sulla-hotfix';
const mime = require('mime-types')
const fs = require('fs');

function start(client: Whatsapp) {
  client.onMessage(async message => {
    if (message.mimetype) {
      const filename = `${message.t}.${mime.extension(message.mimetype)}`;
      const mediaData = await decryptMedia(message);
      const imageBase64 = `data:${message.mimetype};base64,${mediaData.toString('base64')}`;
      await client.sendImage(message.from,imageBase64,filename, `You just sent me this ${message.type}`);
      fs.writeFile(
        filename,
        mediaData,
        function(err) {
          if (err) {
            return console.log(err);
          }
          console.log('The file was saved!');
        }
      );
    }
  });
}

create().then(client => start(client));
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License
[MIT](https://choosealicense.com/licenses/mit/)
