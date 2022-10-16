# Delaying third-party scripts
The solution has been adopted from Gatsby (https://github.com/Kilian/gatsby-plugin-segment-js). 

It works as follows:
- wait for the user to scroll or update the site (for example via clicking any element on the site)
- do a setTimeout for 1 second
- then load the third-party scripts

Due to this time delay, if user visits the website, page appears and starts to be interactive immediately (it depends on first-party scripts though) and the third-party scripts load only if user starts scrolling or making interactions on the page.

It's a perfect solution to load third-parties such as ads, chats, widgets (that are below-the-fold). If user does not make any interaction on site, it will load all delayed 3rd-party scripts after 4s.

## How does it work technically?
All the third-party scripts you want to delay, should use ```<fscript>``` tag instead of ```<script>```. So in case of script such as:
```
<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>(adsbygoogle = window.adsbygoogle || []).push({ google_ad_client: "...", enable_page_level_ads: true });</script> 
```

you should convert it to:

```
<fscript async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></fscript>
<fscript>(adsbygoogle = window.adsbygoogle || []).push({ google_ad_client: "...", enable_page_level_ads: true });</fscript> 
```

This way, the real third-party code is not executed.

Then, the JavaScript code looks for all ```<fscript>``` tags and listens to ```scroll``` or ```click``` events on the website, waits 1 second and converts all the ```<fscript>``` into real ```<script>```, making them executed (wrapped with ```requestIdleCallback``` if supported in user's browser).

## How to use it?
### 1. Add CSS
```fscript {display: none;}```

### 2. Add JS code
See src/index.html in the ```<head>``` section.

### 3. Convert the third-party scripts you want to delay
As explained in the introduction.

Work done :-) 