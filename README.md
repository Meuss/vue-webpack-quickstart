# vue-webpack-quickstart

> just a reminder of some steps for setting up my vue-webpack projects quickly

## Stuff I often include

- Scss
- Vuex
- Eslint
- Prettier (extends eslint)
- Initial templates
- Greensock

### get started
``` bash
vue init webpack project-name
...
cd project-name
npm i sass-loader node-sass -D
npm i vuex gsap -S
```

### .eslintrc.js
``` bash
extends: ['plugin:vue/essential', 'airbnb-base', 'prettier'],
plugins: ['vue', 'prettier'],
...
'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
'prettier/prettier': ['error']
```

### .prettierrc
``` bash
{
  "printWidth": 200,
  "singleQuote": true,
  "trailingComma": "all"
}
```

### store/store.js
``` bash
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  strict: true,
  state: {},
  getters: {},
  mutations: {},
  actions: {},
});
```

### router/index.js
``` bash
import Vue from 'vue';
import Router from 'vue-router';
import Helloworld from '@/components/Helloworld';
import Page404 from '@/components/Page404';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/',
      name: 'helloworld',
      component: Helloworld,
      meta: {
        title: 'Helloworld',
        metaTags: [
          {
            name: 'description',
            content: 'Helloworld',
          },
          {
            property: 'og:description',
            content: 'helloworld',
          },
        ],
      },
    },
    {
      path: '*',
      name: '404',
      component: Page404,
    },
  ],
  mode: 'history',
});
```

### main.js
``` bash
import Vue from 'vue';
import App from './App';
import router from './router';
import store from './store/store';

# others I often use:
# import VueScrollReveal from 'vue-scroll-reveal';

Vue.config.productionTip = false;

# Vue.use(VueScrollReveal, {
#   // https://github.com/scrollreveal/scrollreveal/wiki/Getting-Started-(v3.x)
#   duration: 2000,
#   easing: 'cubic-bezier(0.2, 0.94, 0.11, 1)',
#   scale: 1,
#   opacity: 0.1,
#   distance: '200px',
#   viewFactor: 0.2,
# });

# https://alligator.io/vuejs/vue-router-modify-head/
router.beforeEach((to, from, next) => {
  const nearestWithTitle = to.matched
    .slice()
    .reverse()
    .find(r => r.meta && r.meta.title);
  const nearestWithMeta = to.matched
    .slice()
    .reverse()
    .find(r => r.meta && r.meta.metaTags);
  if (nearestWithTitle) document.title = nearestWithTitle.meta.title;
  Array.from(document.querySelectorAll('[data-vue-router-controlled]')).map(el => el.parentNode.removeChild(el));
  if (!nearestWithMeta) return next();
  nearestWithMeta.meta.metaTags
    .map(tagDef => {
      const tag = document.createElement('meta');
      Object.keys(tagDef).forEach(key => {
        tag.setAttribute(key, tagDef[key]);
      });
      tag.setAttribute('data-vue-router-controlled', '');
      return tag;
    })
    .forEach(tag => document.head.appendChild(tag));
  next();
});

new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>',
});
```
