# Ultimate Progressive Web App CheatSheet

Sample PWA I built using this cheatsheet [TicTacToe PWA](https://www.kelvinkamau.app/AunderwinceTicTacToe/)

## Step One

 * Create a manifest.json in the root folder of your website
 * Add the code snippet below in the file
 * Replace the comments with correct URLs of your web favicon
 * Replace the short_name & name values below with the names of your web application
 
 ```json
{
  "background_color": "#fff",
  "display": "standalone",
  "theme_color": "#fff",

  "short_name": "MyApp",
  "name": "My Progressive Web App",
  "icons": [
    {
      "src": "/* Replace with favicon url i.e images/favicon.png */" ,
      "type": "image/png",
      "sizes": "48x48"
    },
    {
      "src": "/* Replace with favicon url i.e images/favicon.png */" ,
      "type": "image/png",
      "sizes": "72x72"
    },
    {
      "src":  "/* Replace with favicon url i.e images/favicon.png */" ,
      "type": "image/png",
      "sizes": "96x96"
    }
  ,
    {
      "src":  "/* Replace with favicon url i.e images/favicon.png */" ,
      "type": "image/png",
      "sizes": "144x144"
    }
  ,
    {
      "src":  "/* Replace with favicon url i.e images/favicon.png */" ,
      "type": "image/png",
      "sizes": "168x168"
    }
  ,
    {
      "src":  "/* Replace with favicon url i.e images/favicon.png */" ,
      "type": "image/png",
      "sizes": "196x196"
    }
  ,
    {
      "src":  "/* Replace with favicon url i.e images/favicon.png */" ,
      "type": "image/png",
      "sizes": "256x256"
    }

  ],
  "start_url": "index.html?launcher=true"
}
```
## Step Two
* Create a new file in the root folder and name it sw.js
* Copy & Paste the code snippet in the file
* Remember to replace the source files with your own paths in the commented section

```javascript
self.addEventListener('fetch', function(event) {
    event.respondWith(caches.open('cache').then(function(cache) {
        return cache.match(event.request).then(function(response) {
            console.log("cache request: " + event.request.url);
            var fetchPromise = fetch(event.request).then(function(networkResponse) {
                // if we got a response from the cache, update the cache
                console.log("fetch completed: " + event.request.url, networkResponse);
                if (networkResponse) {
                    console.debug("updated cached page: " + event.request.url, networkResponse);
                    cache.put(event.request, networkResponse.clone());
                }
                return networkResponse;
            }, function (e) {
                // rejected promise - just ignore it, we're offline
                console.log("Error in fetch()", e);

                e.waitUntil(
                    caches.open('cache').then(function(cache) {
                        return cache.addAll([
                            '/',

                            '/index.html',
                            '/index.html?homescreen=1',
                            '/?homescreen=1',
                            //Add all your website resoure files here i.e '/index.html',
                        ]);
                    })
                );

            });
            return response || fetchPromise;
        });
    }));
}); 
```
## Lastly : Step Three
* Add the code snippet below in the header section your index.html file 

```html
<head>
    ....
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <link rel="manifest" href="manifest.json">
    ....
</head>
```

* Add the code snippet below just before you close the </body> tag in your index.html file

```javascript
<script>

    if ('serviceWorker' in navigator) {
        window.addEventListener('load', function () {
                navigator.serviceWorker.register('/sw.js')
                    .then(function () {
                        console.log("Service Worker Registered");
                    });
            }
        );
    }
</script>
```
