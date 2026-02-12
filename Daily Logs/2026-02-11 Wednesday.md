1.  Hosted revupro staging project on nginx and server and setup the pm2 process. ssl certificate was expired so fixed that with nilesh bhai. 
2.  review fetch run issue for env on server when running two seperate pm2 process they did not shared the same env so made new ecosystem.config.js file in which gave same env to both pm2 so now it is working. 
[[2026-02-10 Tuesday]]