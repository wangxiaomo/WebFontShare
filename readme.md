# WebFonts && WebFont Loader #

### CSS font-face ###

+ what is @font-face?

  @font-face is a css rule which allows you to download a particular font from your server to render a webpage 
  if the user hasn't got that font installed. This means that web designers will no longer have to adhere to a 
  particular set of "web safe" fonts that the user has pre-installed on their computer.

  ![what is @font-face](https://raw.github.com/wangxiaomo/WebFontShare/master/0.png 'font-face')

+ how do i use @font-face?

  it's pretty simple to implement the @font-face rule, and it is possible to add lots of options to extend its
  features.

    @font-face {
        font-family: DeliciousRoman;
        src: url(http://www.font-face.com/fonts/delicious/Delicious-Roman.otf);
        font-weight: 400;
    }

  then

    p { font-family: DeliciousRoman, sans-serif; }

+ what's the plan for @font-face?

  the final goal: *an online repository of fonts for use with the css rule @font-face - FREE*

other: [font-face.com][0]

### WebFonts ###

[Google WebFonts][1] ---- Google Web Fonts provides high-quality web fonts that you can include in your pages 
using the Google Web Fonts API.

+ Google WebFonts
+ Google WebFonts API

###### Google WebFonts ######

[Google Webfonts Store][2] showing *521* font families.

![Google WebFonts Store](https://raw.github.com/wangxiaomo/WebFontShare/master/1.png 'Google WebFonts Store')

hundreds of free, open-source fonts optimized for web. just 3 quick steps between you and a good lookin' website.

+ Choose
+ Review
+ Use

###### Google WebFonts API ######

![Google WebFonts API][3] helps you add webfonts to any web page. appling a font is easy:
*just add a special stylesheet link to your web page, then use the font in a CSS style*

    <html>
      <head>
        <link rel="stylesheet" type="text/css" href="http://fonts.googleapis.com/css?family=Tangerine">
        <style>
        body { font-family: 'Tangerine', serif; font-size: 48px; }
        </style>
      </head>
      <body>
        <div>Making the Web Beautiful!</div>
      </body>
    </html>

then open the file in a modern web browser. you should see a page displaying the font in Tangerine.

**BUT IN MOST CASES WE NEED WEBFONT LOADER**

### WebFont Loader ###

[WebFont Loader][4] gives you added control when using linked fonts via @font-face. It provides a common 
interface to loading fonts regardless of the source, then adds a standard set of events you may use to control 
the loading experience.

    <script src="http://ajax.googleapis.com/ajax/libs/webfont/1/webfont.js"></script>
    <script>
    WebFont.load({
      google: {
        families: ['Droid Sans']
      }
    });
    </script>
    <style>
    h1 { font-family: 'Droid Sans'; visibility: hidden; }
    .wf-active h1 { visibility: visible; }
    </style>
    <body>
      <h1>This headline will be hidden until Droid Sans is completely loaded.</h1>
    </body>

+ .wf-loading
+ .wf-inactive
+ .wf-active

### SUMMARY && QA ###

ok. 下面开始用中文....

上面的一切看起来是那么美好.不过在我国目前是不可行的.wtf.原因如下:

+ Google WebFonts Store 提供了521种字体.全 TM 是英文字体...
+ 中文字库太大.带宽太小.就算用了 WebFont Loader 依然不会有较好的用户体验.

*TOPIC:*我们要做的就是在无法改变网络状况的前提下缩小字库,没有条件创造条件来提升用户体验.

尽管目前 [http://webfonts.fonts.com][5] 等网站提供了动态缩放字库的在线服务,不过不是缩放效果不好就是会捆绑一些强加
规则.所以我们要开发自己的 TTF Render.

### TTFRender ###

依赖: [fontforge][5]

API Reference: [fontforge scripting][6]

    import fontforge
    from helper import get_ttf_file
    from helper import _unicode

    def generate(token_list, font_name, filename):
        font_file = get_ttf_file(font_name)
        fnt = fontforge.open(font_file)
        for w in fnt.glyphs():
            if w.unicode<0 or \
                    (unichr(w.unicode) not in map(lambda x: _unicode(x), token_list)):
                fnt.removeGlyph(w)
            else:
                print unichr(w.unicode)
        fnt.generate(filename)
        fnt.close()

对内容进行分析后对字库进行缩放,从而有效减少无效字体的存在.

+ 日常文章.

  * 字库 && 字库大小: 楷体 kai.ttf 4.0M
  * 字符数: 1921
  * 缩放后字库大小: 264K

+ 小说

  * 字库 && 字库大小: 楷体 kai.ttf 4.0M
  * 字符数: 7580 3746个不重复字
  * 缩放后字库大小: 1.3M

从以上数据可以粗略的看出在正常情况下缩放效果还是不错的.某些极端情况下生成的字库依然在 M 级别.
这时候我们可以采取 2 级缩放策略.即对常见字缩放一次,对生僻字再缩放一次.

关于浏览器兼容性.主流浏览器均支持 @font-face,兼容性主要体现在字体格式上.IE6/IE7/IE8/IE9 有自身的
font-face 实现,不能使用 TTF/OTF 字体,需要使用 EOT 字体格式.ios4.2 以上的 safari 需要使用 SVG 格式.

最后,目前国内 WebFonts && WebFont Loader 应用并不是很广泛,如果使用妥当的话,必会拔得头筹.

### Author ###

[xiaomo][7]

[wxm4ever@gmail.com][8]

[wxm4ever@douban][9]

---------------------------------

[0]: http://www.font-face.com/ 'font-face.com'
[1]: https://developers.google.com/webfonts/ 'Google WebFonts'
[2]: http://www.google.com/webfonts 'Google Webfonts Store'
[3]: https://developers.google.com/webfonts/docs/getting_started#Quick_Start 'Google WebFonts API Quick Start'
[4]: https://developers.google.com/webfonts/docs/webfont_loader 'Google WebFont Loader'
[5]: http://fontforge.org/ 'fontforge'
[6]: http://fontforge.org/scripting.html#Example 'fontforge scripting'
[7]: http://wangxiaomo.github.com 'wxm'
[8]: mailto:wxm4ever@gmail.com
[9]: http://www.douban.com/people/wxm4ever/ 'wxm@douban'
