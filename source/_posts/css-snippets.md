---
title: css_snippets
date: 2018-01-15 09:32:04
tags: css
---
## A collection of CSS snippets(use [autoprefixer](http://autoprefixer.github.io/) to add vendor)

> gradient for ie8+

```css
  .gradient {
    filter: alpha(opacity=100 finishopacity=20 style=1 startx=0, starty=0, finishy=150) progid:DXImageTransform.Microsoft.gradient(startcolorstr=#BAD1FB, endcolorstr=#E2E9EE, gradientType=0);
    -ms-filter: alpha(opacity=100 finishopacity=100 style=1 startx=0, starty=0, finishx=0, finishy=150) progid:DXImageTransform.Microsoft.gradient(startcolorstr=#BAD1FB, endcolorstr=E2E9EE, gradientType=0);
    background-color: #BAD1FB; /* backforward for old browsers */
    background: -moz-linear-gradient(top, #BAD1FB, #E2E9EE);
    background: -webkit-gradient(linear, 0 0, 0 bottom, from(#BAD1FB), to(#E2E9EE));
    background: linear-gradient(to bottom, #BAD1FB, #E2E9EE); /* autoprefixer will transform it */
  }
```

> shaking effect (update 2018/1/17)
```css
  /* .face element could be a button or anything else even svg */
  .face:hover {
    animation: shake 0.82s cubi-bezier(.36,.07,.19,.97) both;
    transform:translate3d(0,0,0);
    backface-visibility:hidden;
    perspective:1000px;
  }

  @keyframes shake {
    10%, 90% {
      transform:translate3d(-1px, 0, 0);
    }

    20%, 80% {
      transform:translate3d(2px, 0, 0);
    }

    30%, 50%, 70% {
      transform:translate3d(-4px, 0, 0);
    }

    40%, 60% {
      transform:translate3d(4px, 0, 0);
    }
  }

```
Reference: [css-tricks](https://css-tricks.com/snippets/css/shake-css-keyframe-animation/)