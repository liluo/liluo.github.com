---
layout: post
title: "Python 使用 PIL 给图片添加水印"
date: 2011-08-09 22:46
comments: true
categories: [Python, Douban] 
---
前几天在做一个给指定相册添加水印的功能，使用的是PIL(Python Image Library)。
先看一下网上，找到这篇：[Watermark with PIL (Python recipe)](http://code.activestate.com/recipes/362879-watermark-with-pil/) 于是，处理水印的核心代码就差不多有了～

当然，问题也接着来了，首先就是拿到的图片文件和水印文件。我这边得到上传图片文件基本上会是文件二进制数据流或者由Flash post过来的application/octet-stream类型的二进制数据流，并不能像参考中的代码使用指定路径拿到文件，所以数据流进行处理：
``` python
import Image, ImageEnhance
from cStringIO import StringIO
img = Image.open(StringIO(img_data)) # img_data 是post过来的数据流
```
这样就可以拿到一个Image对象了。再使用类似参考代码对它进行水印处理。

<!-- more -->

然后在上传图片的时候，依然使用原来的机制，这里再把加过水印的数据流丢出去：
``` python
new_img = StringIO()
img.save(new_img, img_format, quality=100) # quality来指定生成图片的质量，范围是0～100
reutrn new_img.getvalue()
```

代码上线不久之后，就有同事反映说有png图片上传之后会原图会变掉，我拿png去上传之后效果正常，于是把他那张图拿过来测试，果然报错。后来发现他那张png使用的模式是'RGBA', 而我们一般情况下的png或者gif模式是'P'，会不会是因为图片模式问题呢？测试一下：
``` python
img = Image.open('/Users/luo/p.png')
img.show()
```

在这个时候图片就已经被奇怪的变化了 ＃＃
后来多次尝试发现在 img.save() 时指定format='PNG'时效果正常，于是另加上了mode判断：
``` python
if img.mode != 'RGBA':
    img = img.convert('RGBA')
    img_format = 'JPEG'
else:
    img_format = 'PNG'
img.save(new_img, img_format, quality=100)
```

本地环境测试ok，然后代码提交～
完整代码：

``` python
from cStringIO import StringIO
import Image, ImageEnhance
LEFT_TOP     = 'lt'
LEFT_BOTTOM  = 'lb'
RIGHT_TOP    = 'rt'
RIGHT_BOTTOM = 'rb'

WIDTH_GRID = 30.0
HIGHT_GRID = 30.0

def mark_layout(im, mark, layout=RIGHT_BOTTOM):
    im_width, im_hight     = im.size[0], im.size[1]
    mark_width, mark_hight = mark.size[0], mark.size[1]

    coordinates = { LEFT_TOP: (int(im_width/WIDTH_GRID),int(im_hight/HIGHT_GRID)),
                    LEFT_BOTTOM: (int(im_width/WIDTH_GRID), int(im_hight - mark_hight - im_hight/HIGHT_GRID)),
                    RIGHT_TOP: (int(im_width - mark_width - im_width/WIDTH_GRID), int(im_hight/HIGHT_GRID)),
                    RIGHT_BOTTOM: (int(im_width - mark_width - im_width/WIDTH_GRID), \
                    int(im_hight - mark_hight - im_hight/HIGHT_GRID)),
                  }
    return coordinates[layout]

def reduce_opacity(mark, opacity):
    assert opacity >= 0 and opacity <= 1
    mark  = mark.convert('RGBA') if mark.mode != 'RGBA' else mark.copy()
    alpha = mark.split()[3]
    alpha = ImageEnhance.Brightness(alpha).enhance(opacity)
    mark.putalpha(alpha)
    return mark

def water_mark(img_data, opacity=1):
    img  = Image.open(StringIO(img_data))
    mark = Image.open('/mark/path') # 水印文件可以使用指定路径
    #mark = fs.get(mark_url)

    if not mark:
        return img_data

    mark = Image.open(StringIO(mark))

    if opacity < 1:
        mark = reduce_opacity(mark, opacity)

    if img.mode != 'RGBA':
        img = img.convert('RGBA')
        img_format = 'JPEG'
    else:
        img_format = 'PNG'

    # 指定上传图片最大宽度580和高宽600，如超过进行resize
    if img.size[0] > 580:
        img = img.resize((580, img.size[1]/(img.size[0]/580.0)), resample=1)

    if img.size[1] > 600:
        img = img.resize((img.size[0]/(img.size[1]/600.0),600), resample=1)

    layer = Image.new('RGBA', img.size, (0,0,0,0))
    layer.paste(mark, mark_layout(img, mark, layout))

    img = Image.composite(layer, img, layer)

    new_img = StringIO()
    img.save(new_img, img_format, quality=100)

    return new_img.getvalue()
```
