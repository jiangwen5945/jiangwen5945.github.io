---
title: CSS 常用效果 
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/7.webp'
tags: [Javascript]
categories: [Javascript]
date: 2020-02-28 12:15:00
---
- [制造模糊文本](https://codepen.io/jiangwen5945/pen/EBEree)

  ```css
  .blurry-text {
     color: transparent;
     text-shadow: 0 0 5px rgba(0,0,0,0.5);
  }
  ```

  

- [CSS动画实现省略号动画](https://codepen.io/jiangwen5945/pen/LKdvEx)

  ```css
  .loading:after {
      overflow: hidden;
      display: inline-block;
      vertical-align: bottom;
      animation: ellipsis 2s infinite;
      content: "\2026"; /* ascii code for the ellipsis character */
  }
  
  @keyframes ellipsis {
      from {
          width: 2px;
      }
      to {
          width: 15px;
      }
  }
  ```

  
  
- [CSS悬浮提示文本](https://codepen.io/jiangwen5945/pen/qzozGN)
  
  ```css
  a { 
      border-bottom:1px solid #bbb;
      color:#666;
      display:inline-block;
      position:relative;
      text-decoration:none;
  }
  a:hover,
  a:focus {
      color:#36c;
  }
  a:active {
      top:1px; 
  }
  /* Tooltip styling */
  a[data-tooltip]:after {
      border-top: 8px solid #222;
      border-top: 8px solid hsla(0,0%,0%,.85);
      border-left: 8px solid transparent;
      border-right: 8px solid transparent;
      content: "";
      display: none;
      height: 0;
      width: 0;
      left: 25%;
      position: absolute;
  }
  a[data-tooltip]:before {
      background: #222;
      background: hsla(0,0%,0%,.85);
      color: #f6f6f6;
      content: attr(data-tooltip);
      display: none;
      font-family: sans-serif;
      font-size: 14px;
      height: 32px;
      left: 0;
      line-height: 32px;
      padding: 0 15px;
      position: absolute;
      text-shadow: 0 1px 1px hsla(0,0%,0%,1);
      white-space: nowrap;
      -webkit-border-radius: 5px;
      -moz-border-radius: 5px;
      -o-border-radius: 5px;
      border-radius: 5px;
  }
  a[data-tooltip]:hover:after {
      display: block;
      top: -9px;
  }
  a[data-tooltip]:hover:before {
      display: block;
      top: -41px;
  }
  a[data-tooltip]:active:after {
      top: -10px;
  }
  a[data-tooltip]:active:before {
      top: -42px;
  }
  ```
  
  

- 在可打印的网页中显示URL

  ```css
  @media print   {  
    a:after {  
      content: " [" attr(href) "] ";  
    }  
  }
  ```

  

- CSS font属性缩写

  ```css
  p {
    font: italic small-caps bold 1.2em/1.0em Arial, Tahoma, Helvetica;
  }
  ```

- content属性

  ```css
  
  ```

  

