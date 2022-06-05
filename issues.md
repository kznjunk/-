- [Strapi](#strapi)
- [VueJS](#vuejs)

### Strapi
----------

- Blank page on `/admin`
  - if first time, then `npm run build`
  - verify NGINX config
  - Have a try with the IP instead of DNS
  - `ECONNRESET`: could be no more space on disk


### VueJS
----------

- scroll issue
  - vue 2: "selector"
  - vue 3: "el"

```js
scrollBehavior(to, from, savedPosition) {
  if (savedPosition) {
    return savedPosition
  } else if (to.hash) {
    return { el: to.hash, behavior: 'smooth' } // , offset: { x: 0, y: 10 } }
  } else {
    return { x: 0, y: 0 }
  }
}
```
