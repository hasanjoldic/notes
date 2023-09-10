# `mkcert`

```bash
brew install mkcert
```

Then, create a local certificate authority:

```bash
mkcert -install
```

Create a trusted certificate.

```bash
mkcert {YOUR HOSTNAME e.g. localhost or mysite.example}
```

This create a valid certificate (that will be signed by mkcert automatically).

Configure your development server:

```js
const https = require('https');
const fs = require('fs');
const options = {
  key: fs.readFileSync('{PATH/TO/CERTIFICATE-KEY-FILENAME}.pem'),
  cert: fs.readFileSync('{PATH/TO/CERTIFICATE-FILENAME}.pem'),
};
https
  .createServer(options, function (req, res) {
    // server code
  })
  .listen({PORT});
```

✨ You're done! You can now access https://{YOUR HOSTNAME} in your browser, without warnings
