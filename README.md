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

# Proxy

## axios proxy
- If you use e.g. axios proxy then nock will not work correctly. In order to fix this we create an express server to simulate the proxy
```javascript
before(() => {
    const app = express()
    
    // Middleware zum Parsen von URL-encodierten Daten
    app.use(bodyParser.json({ limit: '50mb' }))
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }))
    
    const storage = multer.memoryStorage()
    const uploadMiddleware = multer({
        storage: storage,
        limits: { fileSize:16*1024*1024 } // accepted limit of file size
    })
    
    // Middleware zum Hochladen des Bildes
    app.use(uploadMiddleware.single('file'))
    
    // Fake proxy server
    app.use(async (req, res) => {
        const { method, url, headers, body, file } = req
        proxyCalledHeaders = headers
        proxyCalled = true
        proxyCalledFile = file
        proxyCalledBody = body
    
        try {
            const response = await axios({
                method,
                url,
                headers,
                data: body
            })
    
            res.status(response.status).send(response.data)
        } catch (e) {
            if (e.response) {
                res.status(e.response.status).send(e.response.data)
            } else {
                res.status(500).send('Internal Server Error')
            }
        }
    })
})


describe('[FormData() - "multipart/form-data"]', () => {
    let form 
    const imageName = "file"
    const path = `${__dirname}/img.png`
    const fileName = `${imageName}.png`

    const pathAfter = `${__dirname}/${imageName}_after.png`

    const nockResponseBody = 'test'

    before(async () => {
        const imageBuffer = await fs.readFile(path)
        
        form = new FormData()
        form.append(imageName, imageBuffer, fileName)
        form.append('name', imageName)
    })

    after(async () => {
        await fs.unlink(pathAfter)
    })

    beforeEach(async () => {
        nock.cleanAll()
        await tdm.insert([datasourceIdWithoutHandlebarsFormData])

        nockInstanceExecuteDatasourceByIdFormData = nockWrapper({
            method: doc_DatasourceIdWithoutHandlebarsFormData.method,
            url: doc_DatasourceIdWithoutHandlebarsFormData.url,
            response: {
                statusCode: 201,
                body: nockResponseBody
            }
        })
    })

    afterEach(async () => {
        await tdm.cleanupAll()
        nock.cleanAll()
    })

    it('should execute datasource by id without variables object and form data', async () => {
        expect(proxyCalled).to.be.equal(false)
        expect(nockInstanceExecuteDatasourceByIdFormData.interceptionCounter).to.be.equal(0)
        expect(doc_DatasourceIdWithoutHandlebarsFormData.headers["Content-Type"]).to.be.equal("multipart/form-data")

        const res = await executeDatasourceById(datasourceIdWithoutHandlebarsFormData, form)
        expect(res.status).to.equal(200)
        expect(res.data).to.equal(nockResponseBody)

        // Save file and compare hash
        fs.writeFileSync(pathAfter, proxyCalledFile.buffer)

        const hash1 = calculateHash(path)
        const hash2 = calculateHash(pathAfter)

        expect(hash1).to.equal(hash2)

        expect(proxyCalled).to.be.equal(true)
        expect(proxyCalledHeaders['content-type']).to.includes('multipart/form-data')
        expect(nockInstanceExecuteDatasourceByIdFormData.interceptionCounter).to.be.equal(1)
    })
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
