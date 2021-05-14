# nock-cheat-sheet
Cheat Sheet for nock with the most needed stuff..



<br><br

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







<br><br
_______________________________________________________

<br><br
