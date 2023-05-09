---
title: "Throwback: Using MomentJS with VuePress."
seoTitle: "MomentJS Integration in VuePress"
seoDescription: "Optimize VuePress static sites with MomentJS for better date formatting and manipulation, enhancing technical documentation and user experiences."
datePublished: Mon May 11 2020 08:34:18 GMT+0000 (Coordinated Universal Time)
cuid: clgywtu22000809jzg3cvg0vw
slug: throwback-using-momentjs-with-vuepress
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/5KNlstQG9DQ/upload/e1da0ee41b2f3a27c8ee712f5ecfe583.jpeg
tags: momentjs, vuepress

---

Originally published: **Monday 11th May 2020.**

A static site is made up of HTML files, CSS files, and JavaScript (JS) files. A static site generator uses raw data (like markdown files) and templates to generate HTML, CSS, and JS files. Let's check out a generator.

## **What is VuePress?**

![](https://images.pexels.com/photos/5641933/pexels-photo-5641933.jpeg?cs=srgb&dl=pexels-anna-shvets-5641933.jpg&fm=jpg&w=640&h=427 align="center")

VuePress is a static site generator that is optimized for creating technical documents using markdown files. It renders a single page app that is powered by Vue, Vue Router, and Webpack. You can even use Vue within your markdown files!

## **What is MomentJS?**

![](https://images.pexels.com/photos/8646394/pexels-photo-8646394.jpeg?cs=srgb&dl=pexels-fauzan-fitria-8646394.jpg&fm=jpg&w=640&h=427 align="center")

[**The MomentJS library**](https://momentjs.com/) is a free ($0), and free (open source), JavaScript wrapper for the Date object (similar in concept to jQuery being a wrapper for the JavaScript language). This library is used to validate, parse, and manipulate dates on the client-side.

## **Why this Post Exists.**

![](https://images.pexels.com/photos/10981241/pexels-photo-10981241.jpeg?cs=srgb&dl=pexels-ann-h-10981241.jpg&fm=jpg&w=640&h=427 align="center")

I am new to VuePress. My previous static site generator was Jekyll (as a result of pushing technical documents to [**GitHub Pages**](https://pages.github.com/)). I switched to VuePress as a direct result of using Vue in my current projects. It's natural to document my Vue adventures with VuePress, yes?

Now I'm thinking: "Gee, that date format looks like crap!! How do I fix it?" My favourite search engine let me down completely ("No shortcuts today, you'll have to figure it out. Beep.") Oh well, here I go...

## **Formating VuePress Dates with MomentJS.**

![](https://images.pexels.com/photos/546819/pexels-photo-546819.jpeg?cs=srgb&dl=pexels-luis-gomes-546819.jpg&fm=jpg&w=640&h=425 align="center")

The first step is to add the MomentJS package to my current VuePress installation.

* From a terminal, I run the following command in the root directory of the VuePress project to download and install the MomentJS component:
    

```bash
$ npm install moment --save
```

> NOTE: The `--save` flag adds an entry to the package.json file.

MomentJS is now ready to use as a VuePress component.

* Create a component called `/docs/.vuepress/components/Date.vue` and add the following:
    

```typescript
<template>
  <span>{{ formatDate($frontmatter.date) }}</span>
</template>

<script>
import moment from 'moment'

export default {
  methods: {
    formatDate(date, format = 'dddd Do MMMM YYYY') {
      moment.locale('en-NZ')
      return (
        moment(date).format(format) +
        ', ' +
        moment(date)
          .startOf()
          .fromNow() +
        '.'
      )
    }
  }
}
</script>
```

* Create a `/docs/test.md` markdown file to test the component:
    

```markdown
---
date: 2020-01-15
---

Date: <Date />
```

> NOTE: I set the locale to 'en-NZ' in `Date.vue`, so the date element in the front matter above follows the YYYY-MM-DD convention.

* From a terminal, run the following command to start the server:
    

```bash
$ vuepress dev docs
```

* Finally, open a browser and visit [**http://localhost:8080/test.html🡕**](http://localhost:8080/test.html%F0%9F%A1%95).
    

I can now drop a `<Date />` component on any page that uses a date element in it's front matter.

> NOTE: The BlogIndex.vue component uses a slightly modified version of the `Date.vue` code listed above.

## **In Conclusion.**

![](https://images.pexels.com/photos/7186207/pexels-photo-7186207.jpeg?cs=srgb&dl=pexels-ann-h-7186207.jpg&fm=jpg&w=640&h=427 align="center")

VuePress (specifically) and Vue (generally) are flexible, adaptable, and performant. They have characteristics that make using Vue-based technologies such a joy.