# BrickLayout晓瀑布流使用指南

## 介绍

BrickLayout 晓瀑布流为使用者提供开箱即用的瀑布流布局的一种可行性的方案，使用者仅需要按照对应所需的字段传入瀑布流组件，即可快速实现瀑布流布局。未来的瀑布流组件将会提供更多样式、适用更多场景的瀑布流模板，敬请期待！希望有更多场景推荐，希望有更多内容定制，欢迎留言或者告诉我们 👏

![组件于未来实验室示例](brickLayout/doc/demo.gif)

## How BrickLayout ✨

由于小程序的诸多限制，导致在 web 上很多常规实现瀑布流的方式大多受到不同程度的影响，小程序中，实现瀑布流组件大抵有两种思路：采用纯粹的 css 来实现，或者通过数据处理配合 css 来实现瀑布流。

* 采用纯粹的 css

  采用纯粹的 css 可以用 multi-column 利用 css3 属性实现多列布局、flex 布局、grid 布局等等。但是结合每个布局的特性，我们率先排除了 grid 布局，因为 grid 布局是实现相对有规则的网格布局，瀑布流布局中，grid 布局不适用。其次，我们排除了 multi-column 这个 css3 属性。在呈现效果上看，multi-column 的确很好地满足了我们对于瀑布流布局的样式布局要求，但是，multi-column 本质上是将文档流分为多列，也就是我们在杂志、报刊常见的多列布局，最后呈现效果（如下图）实际上不满足我们对有序数据的展现要求，因此而排除。

  ![multi-column 展现实际效果](brickLayout/doc/multi-column.png)

  最后我们的考虑范围只剩下 flex 布局，flex 布局在初始状态下的确很好地满足了我们对于数据的呈现效果，但是如果不加以数据干预，在默认非展开的情况下展开单个卡片，极端情况下会导致两列高度差过大，破坏了我们对于瀑布流的要求。

* css 配合数据处理

  综上所述，我们采用了 flex 布局加之对于使用者所传入的数据进行处理，达成了我们想要的效果。前端展现方面，我们还是通过 flex 布局，达成实际的瀑布流呈现效果，对于数据变化亦或是卡片展开时，我们再对数据进行进一步的处理，只有在初始化的时候，或者卡片状态发生改变的时候，对两列高度进行计算，保证两列保持较为稳定的高度差，进而实现瀑布流布局。

  ![brickLayout 展现实际效果](brickLayout/doc/flex.png)

## why BrickLayout ✨

通过 BrickLayout 晓瀑布流使用者无须在关心实际的瀑布流布局实现，也无需关心前端的实际样式布局，更加专注于业务逻辑开发。未来的 BrickLayout 晓瀑布流将为使用者提供更加多样的模板，适用于不同场景之下的瀑布流布局。

  ![brickLayout 展现实际效果](brickLayout/doc/demo.png)

### 使用

1. 在微信小程序管理后台中，按 APPID `wx3c042630f3cdc175` 搜索到该插件，点击添加，即可在代码中使用 `brickLayout`。

2. 在 `app.json` 里，声明该插件的引入，目前该插件为 `0.2.0`，`provider` 为该插件的 APPID，`brickLayout` 为自定义的插件名称。

```json
"plugins": {
  "brickLayout": {
    "version": "0.2.0",
    "provider": "wx3c042630f3cdc175"
  }
}
```

3. 在需要引用瀑布流组件的小程序页面的 `json` 配置文件中，作如下配置：

```json
{
  "usingComponents": {
    "brickLayout": "plugin://brickLayout/brickLayout"
  }
}
```

4. 使用方法

```xml
<brickLayout
  dataSet="{{dataSet}}"
  option="{{brick_option}}"
  bind:tapCard="tapCard"
  bind:tapLike="tapLike"
  bind:tapUser="tapUser"
  bind:onCardExpanded="onCardExpanded"
/>
```

### 组件属性介绍

* **dataSet**
  * **类型：** `Array<Object>`
  * **类别：** required 必填参数
  * **示例值说明：**
    * **id**  
      * 类型： `string`
      * 说明：数据每条记录的唯一标志
    * **content**  
      * 类型：`string`
      * 说明：(optional) 卡片实际记录的内容，不支持解析富文本
    * **backgroundColor**
      * 类型：`string`
      * 说明：(optional) 每个卡片的背景颜色 如果不填，则为随机色。由于字体均为白色，不建议使用白色作为背景色
    * **time**
      * 类型：`unix timestamp`
      * 说明：(optional) 记录的时间戳 如果不填默认没有时间显示
    * **likedCount**
      * 类型：`number`
      * 说明：(optional) 右下角点赞的数量 如果不填默认没有卡片右下角显示
    * **liked**
      * 类型：`bool`
      * 说明：(optional) 如果需要点赞功能，则需要该变量作为用户是否已经点赞的标志，如果为 false 则代表未点赞，icon 为空心 icon，且可以触发点赞动作；如果为 true 则代表已点赞，icon 为实心 icon，且不可再触发点赞动作
    * **user**
      * 类型：`Object`
      * 说明：(optional) 如果不传，则不显示卡片用户区
        * 类型：`string` (optional param) `avatar` 用户头像 url 如果不填默认没有头像
        * 类型：`string` (optional param) `username` 用户名 如果不填默认没有用户名
        * 类型：`string` (optional param) `userId` 用户 id， 如果需要点击用户区域相关事件，可以传入 `userId` ,用于相关点击用户区域事件回调
    * **images**
      * 类型：`Array<string>`
      * 说明：(optional) 如果不传，瀑布流则不显示图片，注意图片是位于文字的下方。默认会一张张显示在文字下面
    * **示例值 🌰：**

```js
[
  {
    id: '1',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
    backgroundColor: '#AF7AC5',
    time: 1533106010,
    likedCount: 0,
    liked: false,
    user: {
      avatar: 'user_avatar_url',
      username: 'Minya Chan',
      userId: '1'
    },
    images: [
       'pic_url', 'pic_url', 'pic_url'
     ]
  },
  {
    id: '2',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
    backgroundColor: '#AF7AC5',
    time: 1533106010,
    likedCount: 0,
    liked: false,
    user: {
      avatar: 'user_avatar_url',
      username: 'Minya Chan',
      userId: '1'
    },
    images: [
       'pic_url', 'pic_url'
     ]
  }
]
```

---

* **option**
  * **类型：** `Object`
  * **类别：** optional 选填参数
  * **示例值说明：**
    * **defaultExpandStatus**
      * 类型： `bool`
      * 说明： (optional) 卡片默认展开情况，默认值为 `false` ，即为默认收起
    * **backgroundColor**
      * 类型：`string`
      * 说明： (optional) 每个卡片默认统一的颜色，如果已经设置了卡片的颜色，优先使用卡片颜色；如果既没有卡片颜色，也没有设置全局背景色，则默认为随机色。即优先级：`card.backgroundColor` > `backgoundColor` > `randomColor`
    * **forceRepaint**
      * 类型： `bool`
      * 说明： (optional) 是否强制重绘，强制重绘则会使原来的展开状态、高度等等全部重置，数据重新渲染。默认值为 `false`，即默认认为源数据的改变只是追加、变更或者减少数据时，不重置卡片的展开状态和高度等等
    * **columns**
      * 类型： `number`
      * 说明： (optional) 瀑布流展示的列数，建议综合考虑一下使用场景的屏幕宽度，默认值为 `2`，如果传入 `0` 是无效的
    * **imageFillMode**
      * 类型： `string`
      * 说明： (optional) 图片的展示功能，详情请参阅 [微信小程序 Image 的 mode 属性](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)，默认值为 `widthFix`
    * **icon**
      * 类型： `Object`
      * 说明： (optional) 支持自定义图标的功能，仅支持上传在线图片 icon, 支持所有 wx.image 组件支持的文件格式。
        * fill：(optional) 卡片点赞后的样式，即原示例中实心的图标样式。
        * default: (optional) 卡片默认图标延时，即原示例中空心的图标样式。
    * **fontColor**
      * 类型：`string`
      * 说明： (optional) 卡片字体颜色
  * **示例值 🌰：**

```js
{
  defaultExpandStatus: false,
  backgroundColor:  '#ababab',
  forceRepaint: false,
  columns: 3,
  imageFillMode: 'widthFix',
  icon:{
    fill:'xxx.com/icon-full.svg',
    default:'xxx.com/icon-default.svg'
  },
  fontColor:'#000'
}
```

---

### 组件事件介绍

如调用的组件的 wxml 声明如下：

```xml
<brickLayout
  dataSet="{{dataSet}}"
  option="{{brick_option}}"
  bind:tapCard="tapCard"
  bind:tapLike="tapLike"
  bind:tapUser="tapUser"
  bind:onCardExpanded="onCardExpanded"
/>
```

* **用户点击区域说明**

![卡片结构说明](brickLayout/doc/sample.jpg)

* **tapCard**
  * **类型：** `function`
  * **类别：** optional 选填参数
  * **说明：** 当用户点击卡片区域时，所触发的自定义事件，可以通过 `event.detail.card_id`拿到对应的卡片的 id，该卡片 id 为 dataSet 里面的唯一标志，唯一标志该数据记录。
  * **示例：**

```js
tapCard: function (event) {
  const cardId =  event.detail.card_id
  // code here.
  console.log('tap card!')
},
```

---

* **tapLike**
  * **类型：** `function`
  * **类别：** optional 选填参数
  * **说明：** 当用户点击点赞区域时，所触发的自定义事件，可以通过 `event.detail.card_id`拿到对应的卡片的 id，该卡片 id 为 dataSet 里面的唯一标志，唯一标志该数据记录。
  * **示例：**

```js
tapLike: function (event) {
  const cardId =  event.detail.card_id
  // code here.
  console.log('tap like!')
},
```

---

* **tapUser**
  * **类型：** `function`
  * **类别：** optional 选填参数
  * **说明：** 当用户点击用户区域（包括 头像、用户名、时间等顶部区域）时，所触发的自定义事件，前提是要传入 `userId`，使用过程中，可以通过 `event.detail.user_id` 拿到所对应的用户 userId
  * **示例：**

```js
tapUser: function (event) {
  const userId =  event.detail.user_id
  // code here.
  console.log('tap user!')
},
```

---

* **onCardExpanded**
  * **类型：** `function`
  * **类别：** optional 选填参数
  * **说明：** 当用户触发展开卡片按钮时，所触发的展开之后的自定义事件，前提是要默认缩起的状态才会有展开和缩起的事件回调，可以通过 `event.detail.card_id` 拿到所对应触发的卡片 id ,该卡片 id 为 dataSet 里面的唯一标志，唯一标志该数据记录；同时还可以通过 `event.detail.expand_status` 拿到当前卡片的展开状态，`true` 代表目前为 `展开` 状态。
  * **示例：**

```js
onCardExpanded:function(event){
  const cardId =  event.detail.card_id
  const expandStatus =  event.detail.expand_status
  // code here
  console.log("expand call back")
},
```

### 版本修改说明 🎉

* 0.1.0
  * init！😜

* 0.1.1
  * 修复空数组提示错误的问题 😢

* 0.2.0
  * 新增图片展示功能，展示于文字下方 😯
  * 新增自定义列数功能 😏
  * 新增图标以及字体颜色自定义 😆
  * 适配 ipad 下的小程序应用 😝
