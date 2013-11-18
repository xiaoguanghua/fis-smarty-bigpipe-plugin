#### Quickling

基于fis支持quickling的smarty插件

#### Overview

插件支持两种渲染模式，

+ 正常渲染模式 `noscript`，不需要前端库进行控制
+ Pipeline渲染模式 `bigpipe`，需要前端库控制渲染

-----

#####`noscript` Render#####

######before######

```html
{%html%}
    {%head%}
        <title>This is a test</title>
    {%/head%}
    {%body%}
        {%style%}
            body {
                background-color: #EEEEEE;
            }
        {%/style%}
        {%script%}
            console.log("foo, foo, foo");
        {%/script%}
        {%widget_block pagelet_id="pager"%}
            {%widget name="widget/a.tpl"%}
            <div>
                //balabala
            </div>
        {%/widget_block%}
    {%/body%}
{%/html%}
```

#####after#####

```html
<html>
    <head>
        <title>This is a test</title>
        <style type="text/css">
            body {
                background-color: #EEEEEE;
            }
        </style>
    </head>
    <body>
        <div id="pager">
            <div>
                I'm `widget/a.tpl`
            </div>
            <div>
                //balabala....
            </div>
        </div>
        <script type="text/javascript">
        !function() {
            console.log("foo, foo, foo");
        }();
        </script>
    </body>
</html>
```

-------

####`bigpipe` render == `pipeline`####

#####before#####

```html
{%html mode="bigpipe"%}
    {%head%}
        <title>This is a test</title>
    {%/head%}
    {%body%}
        {%style%}
            body {
                background-color: #EEEEEE;
            }
        {%/style%}
        {%script%}
            console.log("foo, foo, foo");
        {%/script%}
        {%widget_block pagelet_id="pager"%}
            {%widget name="widget/a.tpl"%}
            <div>
                //balabala
            </div>
        {%/widget_block%}
    {%/body%}
{%/html%}
```

#####after#####

```html
<html>
    <head>
        <title>This is a test</title>
        <style type="text/css">
            body {
                background-color: #EEEEEE;
            }
        </style>
    </head>
    <body>
        <div id="pager">
        </div>
    </body>
</html>
<script type="text/javascript">
BigPipe.onPageReady(function() {
    console.log("foo, foo, foo");
});
</script>
<code style="display:none;" id="__cnt_0"><!--
<div>
    I'm `widget/a.tpl`
</div>
--></code>
<script type="text/javascript">
BigPipe.onPageletArrived({"id":"__elm_0",
"parent_id":"pager","html_id":"__cnt_0"});
</script>
<code style="display:none;" id="__cnt_1">
<!--
<div id="__elm_0"></div>
<div>
    //balabala....
</div>
--></code>
<script type="text/javascript">
BigPipe.onPageletArrived({"id":"pager", 
"html_id":"__cnt_1"});
</script>
<script type="text/javascript">
BigPipe.register({});
</script>
```


