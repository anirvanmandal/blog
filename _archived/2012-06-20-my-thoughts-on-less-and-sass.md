---
layout: post
title: My thoughts on Less and Sass
date: '2012-06-20T23:24:00.003+05:30'
author: Anirvan Mandal
tags:
- css
- LESS
- Sass
- web-design
- ruby-on-rails
modified_time: '2014-01-24T01:34:50.090+05:30'
blogger_id: tag:blogger.com,1999:blog-878378505151338176.post-3555885519497596677
blogger_orig_url: https://static-point.blogspot.com/2012/06/my-thoughts-on-less-and-sass.html
---

I have now worked on two css pre-processor languages, namely [LESS](http://lesscss.org/) and [Sass](http://sass-lang.com/). This blog post is about the differences/similarities between the two languages, the language which according to me is better.

###          Vanilla css vs Preprocessed css
If you are going to build a web based application, then it's plain stupid to go about writing your styles in vanilla css. These are a few things that I feel are the major issues.

- You have to enormous amounts of code.
- Maintaining consistency across the application is a pain because of the lack of variables. For example, say your application css has 20 classes that use a particular width. If you decide to alter the width, then you need to make the same changes in 20 different locations.
- The most granular you can get with vanilla css is a class.

Therefore using a preprocessor language for css is a must in web applications these days. 

###          So what is a css preprocessor ?

Because of the above mentioned shortcomings of css, people created preprocessor languages to facilitate the use of variables, mixins, sprites, functions, mathematical operations, etc. The preprocessor then parses the code into a regular stylesheet.

###          LESS and Sass

LESS and Sass are the most widely used css preprocessors. LESS is written in javascript whereas Sass is written in ruby. Although both of them are available only for ruby projects, an extension has been developed for creating cached stylesheets in php using LESS.

####          Variables

LESS and Sass both facilitate using variables for storing commonly used values thus making it convenient for developers to change the styling of the application as and when needed.

{% highlight css %}
    $height-var = 10px; //Sass
    

    @height-var = 10px; //LESS
{% endhighlight %}

####       Nesting

One of the features provided by css preprocessors that I like the most is nesting. This allows the user to write less, do more.

    
    //Sass and LESS
    .content-box{
      height: 10px;
      width:20px;
      span{
        display: inline-block;
        &:hover{color: white;}
      }
    }
    

    // equivalent vanilla css
    
    .content-box{
        height: 10px;
        width:20px;
    }
    .content-box span{
        display: inline-block;
    }
    .content-box span:hover{
        color: white;
    }
    

Although many people argue that, nesting creates problems/inconsistencies and makes the code harder to read, I find it very efficient and helps me keep the styling modularized.

#### Mixins

Mixins can be thought of as code chunks that can be inherited into any css class or id multiple number of times. This helps us declare commonly used snippets at a particular place and use them wherever needed. This also allows us to keep cross-browser styling together. Take for example inline-block. vendor specific hacks are needed so that inline-block element look the same on different browsers.

    
    display:-moz-inline-stack; 
    display:inline-block;
    zoom:1;
    *display:inline;
    

Therefore for achieving consistency we need to define the above 4 css rules on every inline-block element. LESS and Sass provide mixins for this very purpose.

    //LESS
    
    .display-inline-block {
        display:-moz-inline-stack; 
        display:inline-block;
        zoom:1;
        *display:inline;
    }
    
    
    .box-element{
        .display-inline-block;
    }
    //Sass
    @mixin display-inline-block {
        display:-moz-inline-stack; 
        display:inline-block;
        zoom:1;
        *display:inline;
    }
    
    
    .box-element{
        @include display-inline-block;
    }
    
    

 I personally feel that Sass is more declarative in handling it's mixins. In less a mixin can also be a class and a class can also be a mixin. This basically means that there is no distinction between a class and a mixin unless and until parameters are passed. This can lead to many confusions. Sass on the other hand distinguishes them beautifully and provides a way to cleanly extend classes with it's @extend functionality.

#### Math

Although both the languages provide the functionality of adding mathematical calculations to your stylesheets Sass is more robust in matters of type handling than LESS.

    //LESS
    .box-content{
    
        height: 100px + 20em;// 120px whereas Sass with throw an error in this situation
    
    

#### Compilation

LESS can be compiled on the server-side as well as the client-side. But I found the client-side compilation of LESS to be non-consistent and random. In many cases changes made to the assets take quite some time to take effect. On the other hand Sass can be compiled only on the server-side. This means that every time changes made to the assets will first have to be compiled before deployment. Although this task may take some time,  but it's better than getting random styling on your page.

#### Extendibility

Since LESS is written in javascript, there is little or no scope for providing gems that improve the functionality of LESS in ruby on rails. Sass on the other hand is written in ruby. So gems like compass which provide cross browser mixins, extended functionality can be added to your projects.

#### Compatibility with asset pipeline

Asset pipelining is a framework of ruby on rails that concatenates all the javascript or css files into single master files and also minifies or compresses them. It also helps in fingerprinting assets based on their content. This plays a huge role in content caching thus helping reduce load time of web applications. In LESS I have experienced difficulties implementing asset pipelining for background images for elements. This was mostly due to the lag at compilation on client-side. In Sass asset pipelining works just the it is supposed to. 

####   Development

In terms of development there are :

 92 open issues on LESS

102 open issues in Sass

0 pending pull requests in LESS 

6 pending pull requests in Sass

[Github/Sass](https://github.com/nex3/sass)

[Github/Less](https://github.com/cloudhead/less)

###    Conclusion

Sass is a very declarative language. It is much more robust in terms of performance as compared to LESS. It can be extensible with the help of ruby gems. 

Sass and LESS are the most widely used CSS preprocessor languages. But if you would ask me to choose one, It would certainly be Sass. In fact I have recently shifted an entire project from LESS to Sass.
