# cfp-app

Consumer products are taxed by one or two flat rated VAT.
Our proposal is to include Carbon Foot Print for the item to produce, transport into an additional VAT.
This project demonstrates the calculations of additional CFP based VAT for the scanned products.


Install  nodeenv
----------------
pip install nodeenv

Create a nodeenv
----------------
nodeenv nenv

Start nenv
----------
./nenv/bin/activate


Install @vue/cli
----------------
```
npm install -g @vue/cli

git clone https://github.com/BlacKkNigHT47/CFP-App.git

cd CFP-App

npm install
```
If the above command fails...
```
npm install boostrap-vue

npm i vue-cli-plugin-bootstrap-vue
```

## Project setup
```
npm install

```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

When you build/run if you get the below error, please run the following command
```
 Building for production...Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:67:19)
    at Object.createHash (node:crypto:135:10)
```
```
export NODE_OPTIONS=--openssl-legacy-provider
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
