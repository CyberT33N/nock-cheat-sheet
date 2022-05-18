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
        
# Clear


## cleanAll
- Will clean all interceptors
```javascript
after(() => nock.cleanAll())
```
        
        


















<br><br>
_______________________________________________________
<br><br>
        
## Interceptor

<br><br>

## Remove Interceptors from instance
```javascript
// this will NOT work
const interceptor = nock('http://example.org').get('somePath').reply(200, 'OK')
nock.removeInterceptor(interceptor)

// this is how it should be
const interceptor = nock('http://example.org').get('somePath')
interceptor.reply(200, 'OK')
nock.removeInterceptor(interceptor)  
```
            
        
    <br><br>    
      

## Overwrite response body
```javascript
beforeEach(() => {
    if (nockInstanceSettings) {
        nockInstanceSettings.body = docs_SettingsAll
    } else {
        nockInstanceSettings = nock('http://example.org').get('somePath')
    }
})

it('should return something', async () => {
    nockInstanceSettings.body = [{test:true}]
})
```
            
          
        
        
        
        
        
        
        
        
        
        
        
        
        
        
       
<br><br>
_______________________________________________________
<br><br>
        
## Events


## request
```javascript
nockInstance = nock('https://google.com')
  .get('/')
  
nockInstance.on('request', (req, interceptor) => {
    // ...
})          
```
            
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        


<br><br>
_______________________________________________________
<br><br>
        
Incepter Count
- Check how often your nock instance was called
```javascript
nockInstance = nock('https://google.com')
  .get('/')
                
const interceptorCount = nockInstance.interceptionCounter
```
        
        
        
        
        
        
        
        
        
        



<br><br>
_______________________________________________________

<br><br>
        


# Common Problems

## Nock not working but url seems correct.
- Make sure that your url does not end with /
  - Correct: https://api.web.de/test
  - Not Correct: https://api.web.de/test/
