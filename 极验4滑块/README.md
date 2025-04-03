**本篇文章为作者在学习逆向过程中所做案例的经验分享，本文章中所有内容仅供学习交流，严禁用于商业用途和非法用途，否则由此产生的一切后果均与作者无关，若有侵权，请联系我立即删除！**

**目标接口**

aHR0cHM6Ly9ndDQuZ2VldGVzdC5jb20v

**抓包分析**

**第一个接口**

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzeMLH4cVEkiaKgTX6TBU9sk8BAlrAQLjUmkzYIrq2cR4EAJyhD4PJJmw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

callback   geetest_ + 时间戳

captcha_id   验证码id  是固定值写死

challenge   动态  js生成

client_type   写死

risk_type   写死

lang   写死



响应结果

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzmVvubKLBs0j8ZSI5U0h5zTct0IHcaNLxgYPBMCicZh3z9vLveFs9bXQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

bg   滑块背景缺口图片

lot_number   生成w的关键参数

payload   请求需要携带的参数

datetime   pow_msg需要的参数

process_token 请求verify要携带的参数

**第二个接口**

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGztuAh44hG2hUClLppuJtvYaodvFOibLL0pIrQwP48arJFNfAS1xcEjbA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

callback   geetest_ + 时间戳

captcha_id   验证码id  是固定值写死

client_type   写死

lot_number   由第一个接口生成

risk_type   写死

payload   由第一个接口生成

process_token  由第一个接口生成

w 需要逆向

滑块坐标正确请求成功会返回seccode

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzMzBtnib0lgJMFTpovsUFn19ic2ENxeob03iaOeAyYSYzfQ1l5kibpGlC7A/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

请求失败则没有seccode

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzQbGLQlqRiaxjzl6OLzPbtzY4Trnib5QP885KmXVy32epIhJ1qbcsBHkQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**逆向分析**

**第一个接口参数**

challenge

这种没有接口返回就携带的参数是一开始的js文件生成的，建议先搜索看看有没有值

跟进去

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzdlsUUkYlnM8vzia7GbkgXuPNDTJLS45IbCvCV2N0giaIHeFPOardYulg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

直接贴到本地就能用

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzncBsjcsnribY8Phr9lwRDsA2RLOSY6pgTWg48vFsqWkImicb2WUexvow/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

callback  往上找就能找到，或者直接搜索  一开始就讲过就是 geetest + 时间戳，然后携带这两个参数去python请求就可以得到第二个接口需要携带的参数了

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGz1zTDjrylPFicdMSBfEhl8DAYfzRmqZyxDRzLtHlE2iaicVFqMMLUHyYZA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**第二个接口**

可以慢慢跟栈到这个位置，嫌麻烦可以直接搜这些参数直接定位

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzGLote7IKDk1iaJXFaicFfpszupbLbrAjEcAyqbhqhzwJbyc3Kib7wjJhw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

w是这个东西生成的，进去看看，在返回的位置下个断点

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGz66bicIqibicd8K4J6L5Z8ncCibxe9ugibYUK3TY74qxRvBTFqzmJFmGr7rg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGz19KVLPqZX6jy2khxiaYzfCAHUDrX7RLwAHvvRiclCx3ribIS9uYL6kG9Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzWfC4TI8pfjlRThqXMrphrJoicyRBuXb2sicu0KsHtQ9Sk1jjdDjibW3jQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可以看到是一个函数调用传入u 再加上_

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzgGrrUB3icvCjlciavuLYhpGDSmJowrEO37ZhObN1t4T7Zzf242FCria3Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

先分析u参数，在u处下个断点，一个函数传入一段参数，和一串不知道是啥，发现这串东西是上面的函数调用生成的，进去看看

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzKibicmicMib5bvbXw4AZibwkbqkUicnZO55KicLMQT4UkCSIFkPsia6nPvOHpA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGz7NB9yHIYpJOfprunqr52MC3Rt30oCd6axVa7r7ezhcKBkGZUIlibwIg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

看到这里应该都知道了，这跟三代一个样，4个随机数组成的，扣下来本地用就行，这里有个点要注意，下面要用到这串随机值，但是要保持一致，所以建议写死

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGznIu2Q53I5ia2n4w1rsGZA93cTo0FY8QaQHqZtY69S3eCB3F0sjKYHZA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

再回头分析第一串参数，格式化一下

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGz6whNUIghSNvjRacNkHIE1eNKNxIj0W90W2zUsw7dAlCqbou5QcO4mA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**setLeft   滑动距离**

***\*passtime   滑动需要的时间\****

***\**\*userresponse   滑动距离计算的\*\**\***

***\**\*\*\*lot_number   load接口返回\*\*\*\*\****

***\**\*\*\*\*\*pow_msg   几个数值组成的\*\*\*\*\*\**\***

***\**\*\*\*\*\*\*\*pow_sign   pow_msg加密得来\*\*\*\*\*\*\*\*\****

滑动距离的话用库识别处坐标或者打码都能实现， 滑动花费的时间就是当前时间减去滑块开始滑动的时间，这个值可以写死，userresponse就是滑动距离除以一个定值加2

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzfB1WnTU9GUJMKEkGnK3zAzKBicUE0D6MQfgKNibLx7qicw6eg19K5AV0Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

pow_msg就是一大堆字符串相加 还有一开始的datetime  lot_number 还有随机数

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzO0ZCW4yibicdzK5m63LHCya56rlueB4QBWjl3nct80P4WOoRQlay0EBQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzAqttFuoiaWGLKFauDREoGlXh8YCgia3zQ5HkiaSiasZOTUEMHQN6kls1kQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzEvziaXme8ecT8ibAfJ0QU736gcMPbW7FrbZfxQHbFM7YSlH6LRej9rxw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

pow_sign md5加密，可以自己去对照一下，我就直接说明白这个是标准的md5了，可以调库或者扣，传入的l就是pow_msg

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzUiaian6BibzMH0bDxCWFUVaeNUibgdO4gLiaIqxlqqEOgiatibGWCibGcGgNDQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

传入的参数值都搞完了，接下来分析 u _ 怎么来的了

_值，其实看到65537和 encrypt setPublic可以判断是rsa了，65537同时能判断是标准的rsa，调用了里面的encrypt传入了e()+e()+e()+e() 这个生成的随机数，写死写死保持一致

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzH0icrGkvaBUvhM5mHO5ms9KCDd6ySugc9MxW2w4z8hFp04WGLZSicMPA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzm8VPtNqnBsIWJ6WjbSurWbT5BbAkTAI2CsHNjRicrULD4MnTSC2BDQA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在原型处点进去setPublic进去看看，标准的rsa，到这你可以调库也可以扣代码，扣代码不是很建议，这个混淆有点恶心人，其实如果做过3代或者老版本4代这里是可以取巧的，可以函数暴露导出方法的，函数导出就不用管公钥这些了，_分析完了接下来就是u**
**

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzt64JW9KXRuLCVrehBh0MVabicJypz7APEAtyBb27eJDDzU9cPoOaiaXQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

进去看看，结论就是一个AES加密，传进去生成数组

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzkaicaQLXW2RFJYVDjpaIDEgfDibvYTLRvuqx6gP4VicBkla5liaTlEDz1w/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzszdZysEWgt691wHX92ic4zjEkMN65Xa4L3zNgvTz9G4icXJqzQpL3uag/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzpeXZCB8b3cQbPA5m385kib8MHnMpOeRHyhibabSL28AyIx0EgWuXqnjA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

数组传给生成w值的函数，进去看看，结论就是直接扣下来就行，里面的混淆的都是些JavaScript语法，直接复制过去用就行

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGz1LhpLXJvcp84AIDibibziaogA1k56PBbas5bwyABzerC60qtfrImAb2mQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGzWDyaWPEKxJQ09icFAfN7mfsATDfqadS26nRJA4roDETap0ECeE93Rsw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后逆向分析就结束了，跟python交互好然后识别好把坐标传过去就行，坐标那里还得做做手脚才能过去的，好了结束

**结果验证**

![图片](https://mmbiz.qpic.cn/mmbiz_png/FmagJ3SgEtUFGjeJtC21hhP7HAn6NIGz8a5mXZyqiaNWWjo3dAWCapCnMfEuG4exbkxibZEMBOxSCN8BPPgSvdQg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)