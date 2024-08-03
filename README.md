# Angular

## Replace global Error handler

<https://youtu.be/XJTc-Sy3JxQ>

- create a new error handler
- replace existing error handler using this:

```
(provide:ErrorHandler useClass:MyGlobalErrorhandlerService
```

- Add Stackify.js to get better log information 
- send everything to your backend and log it there

## base href - add url path

### Url Path

```
foo://example.com:8042/over/there?name=ferret#nose
\_/   \______________/\_________/ \_________/ \__/
 |           |            |            |        |
scheme    authority      path        query   fragment
```

Source: <https://angular.io/guide/router#html5-urls-and-the-base-href>

### Compile app to add path

Compile your ng app using `ng build --base-href <path>`

Example: `ng build --base-href lentilsAreTheBest`

Details see: <https://angular.io/cli/build>

The only change I see is in `index.html`:

```html
<head>
  <base href="/lentilsAreTheBest/">
</head>
```

### Usage

`http:\\myDns[:port]\<path>`

### Test base href locally 

That is the only webserver I found, which can provide href option: <https://www.npmjs.com/package/angular-http-server>

>Note: numbers only in base href (like `/1234`) are not working as it seems

Source: <https://dev.to/isthatcentered/testing-an-angular-build-with-base-href-locally-44af>
