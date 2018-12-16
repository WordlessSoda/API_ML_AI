# API_ML_AI

## 产品要求
| 发布日期 | 11/25/2018 |
| --------   | -----:  |
| 史诗 | 菜谱小程序 |
| 文件状态 | 进行中 |
| 文件主人 | Chen |
| 设计者  | Chen |
| 开发者  | Chen |
| 测试者  | Chen |

## 目标
* 根据菜品照片/图片识别出菜品，并给出此菜品的菜谱。
* 根据识别历史记录，推荐与之搭配的菜品方案以及菜谱。


## 背景和战略契合处
* 家中父母时常只会做自己拿手的菜，菜品种类少，即使好吃也因为重复太多而失去食欲。看到好吃的菜品，却不知道菜名，菜谱也就无从查起，希望通过照片/图片识别出菜名，给出详细菜谱，方便父母参照食谱制作菜品，同时三餐的菜品搭配也是一大难题，希望有合理的菜品搭配方案。
* 做菜爱好者希望获取菜谱，自己动手制作菜品。

## 假设
* 用户使用菜品识别，识别生活中拍照所得的菜肴图片，或从微信朋友圈分享得到的菜肴图片，得到菜品名称，以及相应的菜谱。
* 使用python和[百度细粒度图像识别-菜品识别API](https://cloud.baidu.com/product/imagerecognition/fine_grained)和[菜谱查询](http://www.mob.com/product/api/detail/4)

## 加值宣言
菜谱小程序的核心功能基于百度菜品识别API与mob菜谱查询API的调用，为用户提供菜品识别与菜谱查询的实用功能。

## 核心价值
菜谱小程序的核心价值在于识别菜品名称与查询相应的菜谱，帮助用户解决找菜名难，找菜谱难问题，提供菜品与菜谱综合查询的功能。

## 用户痛点
菜谱小程序主要针对以下用户痛点：
* 看到好吃的菜或菜的图片，想知道菜名却不知道怎么搜索。
* 知道了菜名，想要自己动手做，但不知道这道菜的菜谱（需要的食材与制作步骤）。

## 人工智能概率性
在菜品识别和菜谱查询上难免出现失败情况，在菜品识别阶段失败，则给予用户幽默调侃的反馈，在菜谱查询阶段失败，则推荐相似的菜品菜谱。

## 需求
| # | 标题 | 用户案例 | 重要程度 | 笔记 |
| -------- | ----- | ---- | -------- | ----- |
| 1 | 菜品识别 | 用户拍摄上传菜肴图片，识别菜品，显示可能的菜品。 | 重要 | [百度菜品识别技术文档](https://cloud.baidu.com/doc/IMAGERECOGNITION/ImageClassify-API.html#.AA.42.11.B6.D8.DB.EB.F6.75.87.9F.7E.88.AC.D7.60) |
| 2 | 菜谱查询 | 根据用户输入或识别的菜品，列出需要的食材与制作步骤。 | 重要 | [Mob菜谱查询](http://www.mob.com/product/api/detail/4)/[聚合数据菜谱大全（1000次）](https://www.juhe.cn/docs/api/id/46)/[易源数据菜谱大全](https://www.showapi.com/api/view/1164/1)|
| 3 | 菜品搭配推荐 | 为用户推荐与输入或识别的菜品相关的搭配，可以详细列出菜品搭配菜单。 | 一般 |  |


## 使用者交互及设计
> ### [Axure原型展示](https://wordlesssoda.github.io/API_ML_AI_Prototype/)
> 设计中...

### 产品结构图
![ProductStructure](images/PS.png)

### 产品信息结构图
![InformationStructure](images/IS.png)

### 基本页面
![菜品识别页面](images/DR.png)
![菜谱查询页面](images/DM.png)
![个人中心页面](images/MY.png)

## API I/O
### 百度菜品识别API
* Input：一张图片
* Output: 图片的菜品名称、卡路里信息、置信度

> [百度API技术文档](https://cloud.baidu.com/doc/IMAGERECOGNITION/ImageClassify-Python-SDK.html#.E8.8F.9C.E5.93.81.E8.AF.86.E5.88.AB)

#### 输入
![皮蛋瘦肉粥](images/PE.jpg)

#### 输出
```
{
	"log_id": "5608140318891167761",
	"result_num": 5,
	"result": [
		{
			"calorie": "214",
			"has_calorie": true,
			"name": "皮蛋瘦肉粥",
			"probability": "0.921466"
		},
		{
			"calorie": "27",
			"has_calorie": true,
			"name": "海鲜粥",
			"probability": "0.0273665"
		},
		{
			"calorie": "31",
			"has_calorie": true,
			"name": "蔬菜粥",
			"probability": "0.00870583"
		},
		{
			"calorie": "96",
			"has_calorie": true,
			"name": "糖粥",
			"probability": "0.00774943"
		},
		{
			"calorie": "85",
			"has_calorie": true,
			"name": "八宝粥",
			"probability": "0.00583806"
		}
	]
}
```

#### 请求参数
```
""" 读取图片 """
def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()

image = get_file_content('example.jpg')

""" 调用菜品识别 """
client.dishDetect(image);

""" 如果有可选参数 """
options = {}
options["top_num"] = 3
options["filter_threshold"] = "0.7"
options["baike_num"] = 5

""" 带参数调用菜品识别 """
client.dishDetect(image, options)
```
#### 返回示例
```
{
  "log_id": 7357081719365269362,
  "result_num": 5,
  "result": [
  {
    "calorie": "119",
    "has_calorie": true,
    "name": "酸汤鱼",
    "probability": "0.396031"
    "baike_info": {
      "baike_url": "http://baike.baidu.com/item/%E9%85%B8%E6%B1%A4%E9%B1%BC/1754055",
      "description": "酸汤鱼，是黔桂湘交界地区的一道侗族名菜，与侗族相邻的苗、水、瑶等少数民族也有相似菜肴，但其中以贵州侗族酸汤鱼最为有名，据考证此菜肴最早源于黎平县雷洞镇牙双一带。制作原料主要有鱼肉、酸汤、山仓子等香料。成菜后，略带酸味、幽香沁人、鲜嫩爽口开胃，是贵州“黔系”菜肴的代表作之一。这道菜通常先自制酸汤，之后将活鱼去掉内脏，入酸汤煮制。"
    }
  },
  {
    "calorie": "38",
    "has_calorie": true,
    "name": "原味黑鱼煲",
    "probability": "0.265432",

  },
  {
    "calorie": "144",
    "has_calorie": true,
    "name": "椒鱼片",
    "probability": "0.0998993"
  },
  {
    "calorie": "98",
    "has_calorie": true,
    "name": "酸菜鱼",
    "probability": "0.0701917"
  },
  {
    "calorie": "257.65",
    "has_calorie": true,
    "name": "柠檬鱼",
    "probability": "0.0471465"
  }]
}
```
### Mob菜谱查询API
* Input：菜品名称/标签
* Output: 菜谱图片，食材，制作步骤

> [Mob菜谱查询API](http://www.mob.com/product/api/detail/4)

#### 返回示例
```
{
    "msg": "success",
    "result": {
        "curPage": 1,
        "list": [
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015",
                    "0010001031"
                ],
                "ctgTitles": "荤菜,红烧,川菜",
                "menuId": "00100010070000017731",
                "name": "主席红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945926624.jpg",
                    "ingredients": "[\"五花肉一斤半洗净切约2厘米见方的块，葱半两切段，老姜一块拍破，花椒二十余粒，八角四、五个粒，三奈四、五粒，冰糖半两，红烧酱油三大匙，盐适量，鲜汤约二斤。\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945926890.jpg\",\"step\":\"1.将肉放锅内，加入鲜汤（清水也可）置旺火上烧沸，撇去浮沫\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945927086.jpg\",\"step\":\"2.放入葱、姜、花椒、盐、八角、三奈、冰糖、红烧酱油\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945927411.jpg\",\"step\":\"3.改用微火，一直保持沸面不腾，烧至汁浓肉粑，捡去姜、葱不要，起锅即成\"}]",
                    "sumary": "主席红烧肉属于川菜中的一道经典菜，它是四川、重庆许多地区许多人家接待重要客人才做的一道菜。主要原料是猪肉，口味是香，工艺是烧，难度属于中级。",
                    "title": "怎样做好川味经典主席红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945911619.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015",
                    "0010001063"
                ],
                "ctgTitles": "荤菜,红烧,养生",
                "menuId": "00100010070000017714",
                "name": "桂皮红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945871676.jpg",
                    "ingredients": "[\"原料：五花肉、八角、桂皮、香叶、姜、大蒜、冰糖。\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945871961.jpg\",\"step\":\"1.1、五花肉切成麻将大小的方块；入开水中焯一下；另起锅，不放油，直接烧热，下入焯好的五花肉小火煸炒；至肉皮表面变黄并逼出部分油脂，倒入老抽和生抽翻炒上色；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945872208.jpg\",\"step\":\"2.烹入料酒；下入八角、桂皮、香叶、姜、大蒜、冰糖，添加没过的热水；大火烧开后，撇净浮沫；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945872544.jpg\",\"step\":\"3.倒入砂锅大火烧开转小火慢炖；40分钟后添加适量盐，继续小火炖20分钟；开盖转大火收汁，注意不停翻动，至汤汁浓稠，全部裹住肉块且色泽发亮时，关火。\"}]",
                    "sumary": "红烧肉究竟有多少个版本有谁说得清红烧肉究竟有多少个版本？做红烧肉：有人先用冷水煮一下肉，有人先用开水焯一下肉，还有的人把肉直接下锅炒；有人焯水后把肉切块，有人把肉切块后再焯水；有人用糖先炒色，有人用冰糖先炒色，还有的人用老抽来上色；有人先煸炒肉来去除一部分油脂，有人不需要经过煎炒直接下锅煮肉；有人喜欢用这味调料，有人喜欢用那味调料；有人煮1个小时，有人煮2个小时；有人先用铁锅，再转砂锅，有人先用砂锅，再转铁锅；我想，有一百个人做红烧肉，那定会有100种风味。其实，不仅仅是红烧肉。食物，适合自己口味的，才是最好的。我喜欢酱油色味浓重的、透着淡淡的冰糖甜、香而不腻的红烧肉。",
                    "title": "怎样做桂皮红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945845263.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001020"
                ],
                "ctgTitles": "荤菜,炖",
                "menuId": "00100010070000017476",
                "name": "红烧肉炖南瓜",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945092615.jpg",
                    "ingredients": "[\"豆角五花肉1个小南瓜酱油白糖葱丝姜片花椒面\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945092816.jpg\",\"step\":\"1.猪五花切小块，用开水烫一下；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945092920.jpg\",\"step\":\"2.锅内热油入白糖，熬制焦糖色；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945093026.jpg\",\"step\":\"3.将烫过的猪五花入油锅炒上色，加点酱油提色；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945093232.jpg\",\"step\":\"4.将炒好的肉放入阿迪锅把南瓜洗净去瓤，切块放到肉块的上面；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945093430.jpg\",\"step\":\"5.锅调至肉档。要把豆角炒倒，添水加入红烧肉，加入姜片，开锅之后再把南瓜块放入，最后加盐，炖至汤干。\"}]",
                    "sumary": "天冷了，煲点汤，炖点菜吃着热乎乎的暖心暖胃~！爱吃肉的看到这到菜就知道该有多香了，只是做起来略麻烦些，得分两步走哈！",
                    "title": "怎样做红烧肉炖南瓜豆角"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439945057496.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015",
                    "0010001058"
                ],
                "ctgTitles": "荤菜,红烧,养胃",
                "menuId": "00100010070000017386",
                "name": "五香栗子红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944737138.jpg",
                    "ingredients": "[\"主料：五花肉(250克)、带壳板栗(400克)、葱(1根)、大葱(半根)、姜(2片)、蒜粒(3瓣)\",\"香料：八角(1粒)、桂皮(1小块)、干辣椒(10只)、花椒(1汤匙)、月桂叶(2片)\",\"调料：油(3汤匙)、海天铁强化草菇老抽(1\/2汤匙)、海天金标生抽(1汤匙)、盐(1\/4汤匙)、鸡粉(1\/3汤匙)、生粉水(1\/4杯)\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944737330.jpg\",\"step\":\"1.板栗去掉外壳，放入沸水中加盖焖5分钟左右，捞起过冷河沥干水。剥去栗衣，洗净栗子肉;五花肉洗净，切成厚块;葱切段，姜切片，干辣椒切圈;大葱拍扁也切段。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944737647.jpg\",\"step\":\"2.烧热1汤匙油，放入五花肉块，以中小火煸至出油，待其成焦黄色，捞起沥干油，洗净炒锅。烧热2汤匙油，先加入1汤匙花椒粒炒香，捞起弃之;再倒入葱白段、姜片、蒜粒、大葱段和所有香料炒香。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944737938.jpg\",\"step\":\"3.倒入五花肉，与锅内食材一同炒匀，注入2碗清水，以没过五花肉为宜。加入1\/2汤匙海天铁强化草菇老抽和1汤匙海天金标生抽，加盖大火煮沸，改小火焖煮10分钟。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944738533.jpg\",\"step\":\"4.放入栗子肉拌匀，加盖以小火续焖30分钟，煮至栗肉软熟入味。加入1\/4汤匙盐和1\/3汤匙鸡粉调味，淋入1\/4杯生粉水勾芡，洒入葱青段，即可起锅。\"}]",
                    "sumary": "秋天是栗子成熟的季节，栗子味甘，性温，入脾、胃、肾经；具有养胃健脾，补肾强筋，活血止血之功效。五香栗子红烧肉把栗子和红烧肉很好的结合在一起，使各自的美味都得到发挥，是一道秋季养生佳品！",
                    "title": "怎样做五香栗子红烧肉？"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944721587.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015",
                    "0010001063"
                ],
                "ctgTitles": "荤菜,红烧,养生",
                "menuId": "00100010070000017271",
                "name": "红烧肉烧蛋",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944240383.jpg",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944240549.jpg\",\"step\":\"1.猪肉切成2厘米见方的块，最好用五花肉。放入冷水浸泡15分钟，冷水中加入少量料酒喝盐。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944240677.jpg\",\"step\":\"2.猪肉焯水。水烧开，放入猪肉，水开后煮2-3分钟到猪肉变色。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944240850.jpg\",\"step\":\"3.然后把水沥干，猪肉冷水冲干净。焯猪肉的水可以留着，把面上的沫去了备用。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944240923.jpg\",\"step\":\"4.鸡蛋6-8个,煮成白煮蛋。冷水没过鸡蛋,水中加少许盐,水沸后煮5-8分钟,根据你需要的老嫩程度。煮好的蛋趁热加冷水冲一下,会很容易剥壳。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944241088.jpg\",\"step\":\"5.在剥了壳的白煮蛋上用刀竖着划上几刀,这样等下煮起来好入味。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944241250.jpg\",\"step\":\"6.熬冰糖(没冰糖的也可以用红糖)。小火锅中放入冰糖,熬至冰糖融化。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944241406.jpg\",\"step\":\"7.继续熬直至冰糖的颜色变成褐色,有些冒泡为止。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944241574.jpg\",\"step\":\"8.把红烧肉倒入糖浆中,大火快速翻炒直至每块肉都沾上糖浆,肉色变深。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944241646.jpg\",\"step\":\"9.准备大葱、胡萝卜（可省）、欧洲萝卜（可省，也可用土豆代替），青圆椒（可省），都切成块。另外准备八角3个、桂皮少量（我只买到了八角，桂皮这里就省了）。胡萝卜我个人喜欢焯水后使用，此处凭个人喜好。另外，正宗的老上海红烧肉烧蛋是只有肉和蛋组成的，加入蔬菜是我个人的改良。主要家里两口人都不怎么爱吃蔬菜，可是蔬菜沾上肉汤肉汁，突然变的人人都爱吃了。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944241810.jpg\",\"step\":\"10.欧洲萝卜的照片。长的像萝卜,其实口感和土豆基本一样,比土豆更糯一点,完全没一点萝卜的味道,不知道为啥叫欧洲萝卜。当时买它是因为我错把它当成了白萝卜，没想到阴差阳错的反而爱上了它。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944242008.jpg\",\"step\":\"11.把大葱、胡萝卜块、欧洲萝卜块、园椒块和八角桂皮都倒入锅中。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944242211.jpg\",\"step\":\"12.和红烧肉一起翻炒几下。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944242318.jpg\",\"step\":\"13.换锅。先把白煮蛋放入新锅中，再把炒锅里所有的材料倒入，这样蛋在锅子的地步浸在汁水中，更容易焖的入味。有砂锅的最好用砂锅，老上海人认为砂锅煮出来的汤和卤水更香。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944242441.jpg\",\"step\":\"14.锅中加入之前焯猪肉的水或者高汤。我用的是排骨汤。加汤（水）的多少看个人喜好，一般以没过材料为准。我家爱吃汁水多的菜，所以我水加的较多。然后加入少量老抽、糖（较多）、盐。小火炖40分钟到1个小时。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944242605.jpg\",\"step\":\"15.出锅前加入少量鸡精（可省），大火勾芡，出锅。\"}]",
                    "sumary": "浓油赤酱、略带一丝甜味的“老上海红烧肉焖蛋”，好像是寒冬里的一把火，把人的心都烧的暖暖的。此菜可以反复烹制，典型的“饭遭殃”。另外，这个菜里蛋可比肉好吃多了，蛋吃完了可加入新的白煮蛋，其他材料也可反复加煮。第一天煮第二天吃更香。",
                    "title": "怎样自制红烧肉烧蛋"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944217732.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000017243",
                "name": "私房红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944125661.jpg",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944125761.jpg\",\"step\":\"1.五花肉焯水切成1.5厘米左右厚的片状。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944125866.jpg\",\"step\":\"2.配料准备好。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944126100.jpg\",\"step\":\"3.炒锅放少许油，烧至五成热，下蒜瓣爆香。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944126381.jpg\",\"step\":\"4.放五花肉一起爆。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944126672.jpg\",\"step\":\"5.将肥油煸出、表面微焦黄时，倒出多余的油，再倒入其他的配料一起炒香。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944127010.jpg\",\"step\":\"6.依次放入料酒、冰糖、老抽，炒匀，加清水没过肉，大火烧开，转小火焖烧。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944127339.jpg\",\"step\":\"7.至汤汁浓稠，加少许盐和鸡精，大火收汁，直至水分全部收干出油即可。\"}]",
                    "sumary": "和大家分享美食做法，让各位朋友在家就能做出美味菜肴。心动不如行动，感兴趣的朋友们一起动手吧，为家人朋友奉上美味和营养！",
                    "title": "怎样自制私房红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944113789.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000017215",
                "name": "红烧肉团",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944010154.jpg",
                    "ingredients": "[\"食材五花肉淀粉鸡蛋葱姜盐花椒粉酱油蚝油\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944010320.jpg\",\"step\":\"1.把肉剁成肉馅，加入鸡蛋和淀粉，盐，生抽，花椒粉和葱姜水朝一个方向使颈搅抖上劲。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944010452.jpg\",\"step\":\"2.把肉馅团成丸子，下油锅炸至微黄捞出控油。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944010619.jpg\",\"step\":\"3.炸好的丸子加水小火煮一个小时取出。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439944010794.jpg\",\"step\":\"4.炒勺内底油把姜葱炒香，点些老抽酱油，蚝油，盐，糖，加些水再把丸子下入收汁即可。\"}]",
                    "sumary": "肉选三分肥七分瘦的猪肉。最好不用搅肉机搅，用刀剁碎即可。",
                    "title": "怎样做红烧肉团?"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943994702.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001020"
                ],
                "ctgTitles": "荤菜,炖",
                "menuId": "00100010070000017156",
                "name": "萝卜炖红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943703177.jpg",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943703279.jpg\",\"step\":\"1.五花肉切小块，萝卜也切快。（忘拍图了）\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943703430.jpg\",\"step\":\"2.锅中放油，加一汤匙糖，小火炒糖色。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943703539.jpg\",\"step\":\"3.放五花肉翻炒。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943703647.jpg\",\"step\":\"4.放入葱花，姜片翻炒后加一汤匙生抽，老抽适量。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943703800.jpg\",\"step\":\"5.加水莫过肉块，大火烧开，小火炖。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943704276.jpg\",\"step\":\"6.这事炖好的红烧肉，宝宝捣乱，肉有点焦了。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943704381.jpg\",\"step\":\"7.另起锅，放少量的油（红烧肉里还好多油呢）放葱花，蒜片翻炒后放萝卜继续翻炒。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943704543.jpg\",\"step\":\"8.加红烧肉翻炒后，加水，放适量的盐，小火炖到萝卜熟透即可，出锅前放入鸡精。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943704931.jpg\",\"step\":\"9.出锅了，放点香菜。\"}]",
                    "sumary": "和大家分享美食做法，让各位朋友在家就能做出美味菜肴。心动不如行动，感兴趣的朋友们一起动手吧，为家人朋友奉上美味和营养！",
                    "title": "怎样自制萝卜炖红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943656625.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000017007",
                "name": "正宗红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943163643.jpg",
                    "ingredients": "[\"正大五花肉(切块)大葱(切段)大蒜(整瓣)姜片花椒茴香桂皮干辣椒陈皮香叶大料白糖咸盐鸡精料酒陈醋老抽\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943163765.jpg\",\"step\":\"1.五花肉切成1厘米见方,最好带皮\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943163963.jpg\",\"step\":\"2.锅里放油,热后放入白糖,炒到起泡为止,倒入切好的肉,辅料,和干料,大火爆炒1分钟.\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943164163.jpg\",\"step\":\"3.按个人口味加入适量调料:白糖,咸盐,鸡精,料酒,陈醋,老抽.最后加水淹没肉大火煮沸.\"},{\"step\":\"4.大火烧开转小火盖锅盖煮1小时，直到汤呈粘稠状,开大火收汤,不断用铲子翻转,以免干锅,汤汁收快干可以关火焖一下即可.\"}]",
                    "sumary": "上周末做的红烧肉,哈哈,没想到味道这么好,在baidu上搜的正宗红烧肉做法,不过在过程中也有根据自己的情况改动的地方.学以致用,青出于蓝,哈哈.不错不错~香艳又美味儿~",
                    "title": "怎样做正宗红烧肉做法"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943113697.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000016969",
                "name": "三杯红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943039506.jpg",
                    "ingredients": "[\"猪肉半斤\",\"1杯酱油\",\"1杯黄酒\",\"1杯麻油\",\"适量冰糖\",\"适量香料\",\"适量蔬菜\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943039700.jpg\",\"step\":\"1.猪肉切块焯水去除血沫，\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943039905.jpg\",\"step\":\"2.1杯酱油，1杯黄酒，1杯麻油，少许冰糖，少许香料，\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943040106.jpg\",\"step\":\"3.跳到保温键后可放一些蔬菜，再按下煮饭键，再跳起就好了。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439943040239.jpg\",\"step\":\"4.全部放进锅里拌一下，盖上盖，按下煮饭键。\"}]",
                    "sumary": "这个菜是从三杯鸡那里化出来的，并且用电饭锅来煮，放进去就不用管了，等它自动跳转到保温就可以了。实在是方便。",
                    "title": "怎样做三杯红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942991053.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000016911",
                "name": "红烧肉丸子",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942870678.jpg",
                    "ingredients": "[\"猪肉糜300克2支葱2小块生姜0.5小匙盐1小匙老抽1个蛋1大匙淀粉适量调味酱1大匙生抽1小匙香醋1大匙料酒1小匙香油0.5小匙糖2大匙水\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942870890.jpg\",\"step\":\"1.葱洗净切成小葱花，生姜剁成姜米\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942871087.jpg\",\"step\":\"2.鸡蛋打散加到肉糜中，搅拌均匀；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942871274.jpg\",\"step\":\"3.将葱花，姜末，盐，老抽，淀粉加入到肉糜中，顺时针方向搅拌均匀；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942871388.jpg\",\"step\":\"4.锅内放适量油（能煎肉丸子那么多就成），将猪肉糜用勺子或者手捏成比乒乓球稍小的丸子，一个个放到锅里煎至熟透金黄，捞出沥油；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942871498.jpg\",\"step\":\"5.生姜切丝备用，将调味酱里所有的材料除淀粉和２大勺水以外放到一个容器里搅拌均匀；\"},{\"step\":\"6.锅内留少许油，比平常炒菜稍少些就成，烧热，下姜丝爆出香味，将炸好的肉丸子全部放下去，翻炒几下，将调好的酱汁倒入锅里，改小火，关上锅盖焖烧几分钟，用淀粉和２大勺水勾芡，转大火，收浓汤汁即可出锅。\"}]",
                    "sumary": "每次饿得肚子咕咕叫的时候，总是满脑子红烧狮子头。每次吃这道菜，怎么都觉得是美味。ＬＧ说，“看来你就是爱吃肉。”爱吃就爱吃吧，也没啥关系。红烧狮子头做起来比较麻烦些，平日里就弄点红烧肉丸子解解谗吧。",
                    "title": "怎样做红烧肉丸子"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942856609.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000016878",
                "name": "腊笋红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942766115.jpg",
                    "ingredients": "[\"红烧肉\",\"腊笋\",\"3大勺料酒\",\"5大勺老抽\",\"2大勺糖\",\"2大勺盐\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942766370.jpg\",\"step\":\"1.腊笋很硬，要用凉水泡两天，泡两天它就发胖发白了。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942766605.jpg\",\"step\":\"2.泡发后切丝(这可是个体力活)用高压锅煮一下，冒气兹滋响后煮5分钟就可以了。然后用锅里的水在泡两天。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942766719.jpg\",\"step\":\"3.先要把红烧肉做成6成熟,这个就简约过去不在介绍了。锅中放油，如果有大油的话最好，这个腊笋很吃油的。放入腊笋丝，翻炒。翻炒的过程中放料酒3大勺，老抽5大勺。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942766972.jpg\",\"step\":\"4.在放入2大勺糖，不挺的翻炒，大约10分钟的样子，把红烧肉放入。在翻炒3分钟，锅中放水末过菜，盖上锅盖炖煮3个小时。炖半个小时后放2大勺子盐。\"}]",
                    "sumary": "春节的时候去老公家，吃到婆婆做的腊笋红烧肉，自己非常的爱吃，回来的时候带回一些自己尝试做了一回，非常的麻烦，但是非常成功，口味和家里做的一样好吃。这个腊笋就是竹子出生的时候，嘿嘿，已经晒成干。",
                    "title": "怎样做腊笋红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439942752632.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000016487",
                "name": "红烧肉翅",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439941528865.jpg",
                    "ingredients": "[\"肥瘦相间的猪肉、鸡翅中、冰糖、生姜、酒糟、红烧酱油、蚝油、生抽、盐。\"]",
                    "method": "[{\"step\":\"1.猪肉切成大小均匀的小块，鸡翅中表面浅划几刀，姜切片。把猪肉、鸡翅用酒糟、生姜加点水腌1小时。\"},{\"step\":\"2.腌汁倒掉，把腌好的猪肉、鸡翅捞出冲洗干净，连同生姜片倒入高压锅内，注入清水（水不能太多，没过肉的2\/3处即可；水多汤不鲜）。\"},{\"step\":\"3.盖上高压锅盖，大火烧至高压锅冒气后转中小火烧10-15分钟。开盖，到入冰糖、红烧酱油、蚝油、生抽调味上色。\"},{\"step\":\"4.收汁，尝尝咸淡，酌情加盐。收汁时，要手摇高压锅不停晃动，让食物均匀入味上色。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439941529261.jpg\",\"step\":\"5.\"}]",
                    "sumary": "",
                    "title": "怎样做红烧肉翅"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439941503983.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000016206",
                "name": "香菇脚红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940631778.jpg",
                    "ingredients": "[\"500g五花肉\",\"一把香菇脚\",\"200G笋干\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940632118.jpg\",\"step\":\"1.五花肉煮至八成熟，切成小块\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940632313.jpg\",\"step\":\"2.笋干煮半小时，泡一天\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940632626.jpg\",\"step\":\"3.香菇脚，泡发\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940632840.jpg\",\"step\":\"4.糖炒化后，放入五花肉块，迅速翻炒，尽量让每块肉都裹上糖，放入大片姜片，\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940632952.jpg\",\"step\":\"5.五花肉出油后，加盐、酱油、水换成砂锅煮\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940633141.jpg\",\"step\":\"6.中小火把水烧一半时，加入笋干，用小火再慢慢煮\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940633997.jpg\",\"step\":\"7.不断搅动，直至油大多出来\"}]",
                    "sumary": "今天烧红烧肉，里面也放了些香菇脚哦。糖炒化后，放入五花肉块，迅速翻炒，尽量让每块肉都裹上糖，放入大片姜片，香菇脚用开水泡开后，并去掉不能吃的部份，红烧肉放入八角翻炒八角的香味出来后，放入香菇脚，烧一会，放入料酒和生抽，加入开水（一定要热水，不能放冷水），加盐盖上盖，让红烧肉烧一会，准备好砂锅，水最好和肉平行，加少许老抽，为了让成品颜色更好看，大火烧开，转中火，中火烧至水只有原来一半的时候，加入泡过切好的笋干，大火再次烧开，转小火，继续烧，随着水慢慢少，从每三分钟翻一次，到每半分钟翻一次，直到水份消失，红烧肉的油开始出来，以不焦为基础，红烧肉能尽量多出油，这样的标准，做出的红烧肉入口即化，而且还不油腻。",
                    "title": "怎样做香菇脚红烧肉？"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439940587163.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000015375",
                "name": "红烧肉夹馍",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439937999886.jpg",
                    "ingredients": "[\"馅料：红烧肉、辣椒、香菜\",\"饼料：面粉、酵母、水\"]",
                    "method": "[{\"step\":\"1.酵母用30度左右的水稀释，静置3分钟；\"},{\"step\":\"2.添加面粉，用筷子搅拌成湿面絮；\"},{\"step\":\"3.揉成光滑的面团，盖上保鲜膜放温暖处饧发；\"},{\"step\":\"4.面团饧发至2.5倍大小，取出揉匀排气；\"},{\"step\":\"5.分割成大小均匀的小面团；\"},{\"step\":\"6.揉匀，擀成圆饼，盖上湿布再次饧发；\"},{\"step\":\"7.待饼胚饧发至蓬松轻盈状，移入饼铛，反正面烙熟即可；\"},{\"step\":\"8.红烧肉、辣椒和香菜切碎拌匀；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439938000150.jpg\",\"step\":\"9.面饼从中间分割，不要切断，填满馅料，夹而食之。\"}]",
                    "sumary": "这红烧肉夹馍，被我称之为红烧肉的升级版吃法。都说红烧肉和米饭是绝配，可红烧肉拌上辣椒和香菜，切碎之后夹于馍内食之，那滋味不是单吃红烧肉可同日而语的。确实是滋味和享受又上升了一个层次！",
                    "title": "怎样在家做红烧肉夹馍"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439937978372.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000014898",
                "name": "传统红烧肉",
                "recipe": {
                    "sumary": "红烧肉是一道常见的家常菜，深受大家喜爱，一起看看这道菜的做法吧，或许会让你另有收获哦。",
                    "title": "怎样做传统红烧肉？"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439936570922.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000014852",
                "name": "板栗竹笋红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439936467238.jpg",
                    "ingredients": "[\"五花肉500克、冰糖20克、栗子仁随意、八角2个、香叶1片、花椒10粒、桂皮1块、葱姜蒜各2片。\",\"老抽2汤勺、生抽2汤勺、盐2茶匙、料酒2汤勺。\"]",
                    "method": "[{\"step\":\"1.五花肉洗净去毛，冷水浸泡5-10分钟。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439936467807.jpg\",\"step\":\"2.切成约3x3cm左右大小的块。\"},{\"step\":\"3.凉水入锅，放入五花肉、葱、姜，加入料酒，大火烧开。\"},{\"step\":\"4.水开后，撇去浮沫，捞出五花肉控干水分备用。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439936469098.jpg\",\"step\":\"5.煎锅点小火，放入五花肉，把肉煎出油来。煎完一面再翻面重复。\"},{\"step\":\"6.煎出油后，放入冰糖，继续小火融化。淋入1汤勺老抽和2汤勺生抽翻炒均匀。\"}]",
                    "sumary": "在冷风呼啸的冬季里面，捧着刚出锅的糖炒栗子，是一件我觉得非常非常让人幸福的事情。如果家里的糖炒栗子还没有吃完，就顺手剥几个放在炖肉里面吧。栗子仁吸收肉的香味，也会变得好吃无比。",
                    "title": "板栗竹笋红烧肉的做法"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439936434959.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015"
                ],
                "ctgTitles": "荤菜,红烧",
                "menuId": "00100010070000014671",
                "name": "茨菇红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935756909.jpg",
                    "ingredients": "[\"原料：茨菇、五花肉、海天草菇老抽、冰糖适量、黄酒适量、大葱白两小段、姜两片、八角（大料）一个、香叶一片。\"]",
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935757366.jpg\",\"step\":\"1.先焯水，然后把猪皮上的毛在火上烤掉,一定要将红烧肉皮上的毛处理干净，个人比较在意这个，呵呵\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935758014.jpg\",\"step\":\"2.将肉清洗干净控干水分，控干水分后将肉有规律的切块；茨菇处理干净，控干水分备用；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935758257.jpg\",\"step\":\"3.将锅中放少许油，油热后将姜片放入略煸，将敲碎的冰糖放入，翻炒至融化。\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935758635.jpg\",\"step\":\"4.五花肉入锅煸炒，待见肥肉部分开始出油，肉的整体变白时，将茨菇也放入加水至没过所有主料，加适量老抽上色，将葱段、八角、香叶都放入，盖锅盖，大火煮开，若有浮沫最好撇去~转中小火煮；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935761989.jpg\",\"step\":\"5.待煮到肉酥烂程度差不多了，开盖，此时汤汁也比较少了，转大火，用少许鸡精、盐调味，将汤汁大火收至略变粘稠时即可关火装盘了。\"}]",
                    "sumary": "红烧肉是热菜菜谱之一，以五花肉为制作主料，红烧肉的烹饪技巧以砂锅为主，口味属于甜味。红烧肉是一道著名的本帮菜，充分体现了本帮菜“浓油赤酱”的特点。",
                    "title": "新春美食茨菇红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935746834.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015",
                    "0010001034"
                ],
                "ctgTitles": "荤菜,红烧,浙菜",
                "menuId": "00100010070000014641",
                "name": "清淡红烧肉",
                "recipe": {
                    "img": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935659272.jpg",
                    "ingredients": "[\"五花肉，葱，姜，酱油，料酒，水，白糖，醋。\"]",
                    "sumary": "红烧肉好吃难做，现为大家介绍一款好吃不腻的红烧肉做法。",
                    "title": "怎样做清淡红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935618398.jpg"
            },
            {
                "ctgIds": [
                    "0010001007",
                    "0010001015",
                    "0010001035"
                ],
                "ctgTitles": "荤菜,红烧,湘菜",
                "menuId": "00100010070000014616",
                "name": "湖南风味红烧肉",
                "recipe": {
                    "method": "[{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935573691.jpg\",\"step\":\"1.1、将带皮猪肉切成块放入；\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935573819.jpg\",\"step\":\"2.加入酱油、糖，搅动等它自己出油\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935574049.jpg\",\"step\":\"3.加入桂皮八角茴等香料，继续搅动\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935574314.jpg\",\"step\":\"4.可以再加些酱油\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935574622.jpg\",\"step\":\"5.肉已经差不多啦，这时候可以放入香干或者干豆荚什么的\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935574927.jpg\",\"step\":\"6.肉已经差不多啦，这时候可以放入香干或者干豆荚什么的\"},{\"img\":\"http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935575154.jpg\",\"step\":\"7.可以享用了\"}]",
                    "sumary": "带皮五花肉，葱，姜，酱油，料酒，白糖，花椒，大料，香叶，炖肉粉。",
                    "title": "怎样做湖南风味红烧肉"
                },
                "thumbnail": "http:\/\/f2.mob.com\/null\/2015\/08\/19\/1439935559434.jpg"
            }
        ],
        "total": 151
    },
    "retCode": "200"
}
```

## 问题
| 问题 | 结果 |
| -------- | ----- |
| 我如何知道这道菜的名字？ | 拍摄或上传图片，识别菜品的名称。 |
| 我已经知道了菜的名字，但我想学怎么做？ | 根据菜名给出菜谱。 |
| 我不知道该怎么搭配这道菜？ | 推荐与之合理搭配的菜单。 |

## 还没做
* 根据用户选择口味，筛选推荐菜谱搭配。
* 用户分享菜肴制作过程视频，文章功能等。
