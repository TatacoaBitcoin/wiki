---
title: 'Gatsby'
metaTitle: 'Guías - Gatsby'
metaDescription: 'How-to guides'
---

# Website deployment

1. Clone/pull repo

```
$ git clone https://github.com/truxxu/gatsby-blog-demo.git
$ cd gatsby-blog-demo/
$ git pull
```

2. Build site

```
$ npm install
$ npx gatsby build
```

3. Make backup of current deployed site

```
$ ssh root@83.97.20.214
# cp -r /var/www/html/www.tatacoabitcoin.com /var/www/html/www.tatacoabitcoin.com.back
```

4. Deploy

```
$ scp -r public/\* root@83.97.20.214:/var/www/html/www.tatacoabitcoin.com/

```
