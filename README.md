# Zeit-Now-deploy
Deployment, aliasing and redirecting with Zeit Now

1) Make a run build of your project (bundle)

2) Set a now.json file with:
  {
    "name": "buiap",     // the prefix before now.sh => prefix.scope.now.sh 
    "scope": "stefafrontani",      // the scope to use when deploying => prefix.scope.now.sh 
    "alias": "buiap.com",      // the alias to use when deploying to particular domain
    "version": 2,      // avoiding warning when deploying with NOW
    "public": true,      //
    "routes": [
      { "src": "/app.bundle.js", "dest": "/app.bundle.js" },
      { "src": "/offline.js", "dest": "/offline.js" },
      { "src": "/manifest.json", "dest": "/manifest.json" },
      { "src": "^((?!static).)*$", "dest": "/" },
      { "src": "/", "dest": "/index.html" } 
    ] .     // When using Single Web Applications, every request that comes from domain:port/* must return the same "/" file, the html. Other static assests such as app bundle, images in static file, manifest, offline service worker must be explicitly return
  }
  
  3) Move to the location when you have the dist (build) folder and run now. this will create a url for deploy and alias to:
    [buiap].[stefafrontani].[now.sh] 
    [name-in-json].[scope-in-json].[suffix]
    
  4) if you want to deploy with a domain, you should add the alias in json
  
  5) now domain add www.my-domain.com
 
  6) now domain inspect www.my-domain.com // it gives you some nameservers wich you have to add where the domain is register
  
  7) after some time, now will detect the nameservers added and will enable you to ALIAS your deployment. 
  8) You deploy like before: 
    now 
  9) You alias the url given by (8) like: 
    now alias url-in-step-eight alias-domain => https://name-in-json.scope-in-json.now.sh my-domain.com
  
  10) Once the deploy is ready, www.my-domain.com will not be available. You have to redirect form www to => my-domai.com
  
  11) Create a folder "redirect" with a "now.json" file in it
  
  12) Paste:
    {
      "version": 2,
      "alias": ["www.buiap.com"],
      "routes": [
        {
          "src": "/(.*)",
          "status": 301,
          "headers": { "Location": "https://buiap.com/$1" }
        }
      ]
    }
    
  13) Deploy with now the folder in step (12) writing "now" in terminal, once you are in the folder's directory
  
  14) Take the url create by step (13) and alias to www.my-domain.com
    now alias https://redirect.username.now.sh www.buiap.com
    
  15) DONE
  
/*  REDIRECT FILE */
source: https://zeit.co/guides/redirect-from-www/
