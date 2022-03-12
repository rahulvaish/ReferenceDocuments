### NGINX LoadBalancing

##### STEP#1: SetUp NGINX
```
brew install nginx
nginx -v
```

##### STEP#2: Edit nginx.conf as below
```
cd /opt/homebrew/etc/nginx
open -a TextEdit nginx.conf
```
```
http {
   upstream mybackend {
        #Round Robin - default
        server 127.0.0.1:1111;
        server 127.0.0.1:2222;
        server 127.0.0.1:3333;
        server 127.0.0.1:4444;
        server 127.0.0.1:5555;
        server 127.0.0.1:6666;
   }
   server {
     listen 5050;
     location / {
         proxy_pass http://mybackend;
     }
   }
}
events {}
```

##### STEP#3: Start n SpringBoot/Node servers with ports as 1111,2222,3333,4444,5555,6666
<img width="1610" alt="image" src="https://user-images.githubusercontent.com/689226/155869080-833357a2-eed9-4c29-96d8-95fd69dfc2c6.png">

###### Example:

```
const express = require('express');
const app = express();
const port = 6666;

app.get('/', (req, res) => {
  res.send({
      message: "Hello, I'm Server Six"
  })
})

app.listen(port, () => {
  console.log(`Server runnig listening at http://localhost:${port}`);
})

```

##### STEP#4: Test
<img width="644" alt="image" src="https://user-images.githubusercontent.com/689226/155869726-ef53b251-bb6c-4996-aa10-68f1cd2df794.png">

<img width="693" alt="image" src="https://user-images.githubusercontent.com/689226/155869744-6fcf3278-ea6a-4689-818f-aac8c0de99f1.png">


events {}
