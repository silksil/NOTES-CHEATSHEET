
``` javascript
import Vue from 'vue';
import Router from 'vue-router';
import componentName from '@/components/componentName.vue';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/componentRoute',
      name: 'componentName',
      component: componentName
    },
  ],
});

```
