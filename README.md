# nock-cheat-sheet
Cheat Sheet for nock with the most needed stuff..



<br><br>

# Simulate Error
```javascript
const nock = require('nock')
const axios = require('axios')

nock('https://google.com')
  .get('/')
  .replyWithError('something awful happened');
  // .reply(500, 'FAILED!');

async function test() {
  try {
    const res = await axios.get('https://google.com/')
  }
  catch (e) {
    console.log(e)
  }
}

test()
```










<br><br>
_______________________________________________________
<br><br>
        
Clear
```javascript
after(() => nock.cleanAll())
```
        
        
        
        
        
        
        
        
        
        
        
        
        
        


<br><br>
_______________________________________________________
<br><br>
        
Incepter Count
- Check how often your nock instance was called
```javascript
nockInstance = nock('https://google.com')
  .get('/')
                
const interceptorCount = nockInstance.interceptors[0].interceptionCounter
```
        
        
        
        
        
        
        
        
        
        



<br><br>
_______________________________________________________

<br><br>
        


# Common Problems

## Nock not working but url seems correct.
- Make sure that your url does not end with /
  - Correct: https://api.web.de/test
  - Not Correct: https://api.web.de/test/
