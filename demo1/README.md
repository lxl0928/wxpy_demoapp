# 每天不同时间段通过微信发消息给女朋友

## 简介
通过一个定时py脚本，发送指定微信消息给女朋友。

## 开发思路
使用python中的wxpy模块完成微信的基本操作。

设置一个**config.ini**配置文件，并从这个配置文件开始读取信息。

### 读取配置文件

```python
cf = configparser.ConfigParser()
cf.read("./config.ini",encoding='UTF-8')
```

### 设置女朋友的微信名称，记住，不是微信ID也不是微信备注

```python
girl_friend_wechat_name = cf.get("configuration", "friend_wechat_name")
```

### 设置早上起床时间，中午吃饭时间，下午吃饭时间，晚上睡觉时间

```python
say_good_morning = cf.get("configuration", "say_good_morning")
say_good_lunch = cf.get("configuration", "say_good_lunch")
say_good_dinner = cf.get("configuration", "say_good_dinner")
say_good_dream = cf.get("configuration", "say_good_dream")
```

### 设置女朋友生日信息
```python
birthday_month = cf.get("configuration", "birthday_month")  # 几月，注意补全数字，为两位数，比如6月必须写成06

birthday_day = cf.get("configuration", "birthday_day") # 几号，注意补全数字，为两位数，比如6号必须写成08
```

### 早上起床问候语列表，数据来源于新浪微博
```python
str_list_good_morning = ''
with open("./remind_sentence/sentence_good_morning.txt", "r",encoding='UTF-8') as f:
    str_list_good_morning = f.readlines()
print(str_list_good_morning)
```

### 中午吃饭问候语列表，数据来源于新浪微博
```python
str_list_good_lunch = ''
with open("./remind_sentence/sentence_good_lunch.txt", "r",encoding='UTF-8') as f:
    str_list_good_lunch = f.readlines()
print(str_list_good_lunch)
```

### 晚上吃饭问候语列表，数据来源于新浪微博
```python
str_list_good_dinner = ''
with open("./remind_sentence/sentence_good_dinner.txt", "r",encoding='UTF-8') as f:
    str_list_good_dinner = f.readlines()
print(str_list_good_dinner)
```

### 晚上睡觉问候语列表，数据来源于新浪微博
```python
str_list_good_dream = ''
with open("./remind_sentence/sentence_good_dream.txt", "r",encoding='UTF-8') as f:
    str_list_good_dream = f.readlines()
print(str_list_good_dream)
```

### 设置晚上睡觉问候语是否在原来的基础上再加上每日学英语精句(False表示否 True表示是)
```python
if((cf.get("configuration", "flag_learn_english")) == '1'):
	flag_learn_english = True
else:
	flag_learn_english = False
print(flag_learn_english)
```

### 设置所有问候语结束是否加上表情符号(False表示否 True表示是)
```python
str_emoj = "(•‾̑⌣‾̑•)✧˖°----(๑´ڡ`๑)----(๑¯ิε ¯ิ๑)----(๑•́ ₃ •̀๑)----( ∙̆ .̯ ∙̆ )----(๑˘ ˘๑)----(●′ω`●)----(●･̆⍛･̆●)----ಥ_ಥ----_(:qゝ∠)----(´；ω；`)----( `)3')----Σ((( つ•̀ω•́)つ----╰(*´︶`*)╯----( ´´ิ∀´ิ` )----(´∩｀。)----( ื▿ ื)----(｡ŏ_ŏ)----( •ิ _ •ิ )----ヽ(*΄◞ิ౪◟ิ‵ *)----( ˘ ³˘)----(; ´_ゝ`)----(*ˉ﹃ˉ)----(◍'౪`◍)ﾉﾞ----(｡◝‿◜｡)----(ಠ .̫.̫ ಠ)----(´◞⊖◟`)----(。≖ˇェˇ≖｡)----(◕ܫ◕)----(｀◕‸◕´+)----(▼ _ ▼)----( ◉ืൠ◉ื)----ㄟ(◑‿◐ )ㄏ----(●'◡'●)ﾉ♥----(｡◕ˇ∀ˇ◕）----( ◔ ڼ ◔ )----( ´◔ ‸◔`)----(☍﹏⁰)----(♥◠‿◠)----ლ(╹◡╹ლ )----(๑꒪◞౪◟꒪๑)"
str_list_emoj = str_emoj.split('----')
if ((cf.get("configuration", "flag_wx_emoj")) == '1'):
	flag_wx_emoj = True
else:
	flag_wx_emoj = False
print(str_list_emoj)
```

### 设置节日祝福语
#### 情人节祝福语
```python
str_Valentine = cf.get("configuration", "str_Valentine")
print(str_Valentine)
```

#### 三八妇女节祝福语
```python
str_Women = cf.get("configuration", "str_Women")
print(str_Women)
```

#### 平安夜祝福语
```python
str_Christmas_Eve = cf.get("configuration", "str_Christmas_Eve")
print(str_Christmas_Eve)
```

#### 圣诞节祝福语
```python
str_Christmas = cf.get("configuration", "str_Christmas")
print(str_Christmas)
```

#### 生日的时候的祝福语
```python
str_birthday = cf.get("configuration", "str_birthday")
print(str_birthday)
```

可以在上面对时间的判断中，加入一些其他你想要的，这样你女朋友就更开心啦！后期如果有时间，我将会加上以上节日问候功能。😀




接着，开启微信机器人，为了程序的健壮性，自动判断一下操作系统，根据不同操作系统执行不同指令
```python
# 启动微信机器人，自动根据操作系统执行不同的指令
# windows系统或macOS Sierra系统使用bot = Bot()
# linux系统或macOS Terminal系统使用bot = Bot(console_qr=2)
if('Windows' in platform.system()):
    # Windows
    bot = Bot()
elif('Darwin' in platform.system()):
    # MacOSX
    bot = Bot()
elif('Linux' in platform.system()):
    # Linux
    bot = Bot(console_qr=2,cache_path=True)
else:
    # 自行确定
    print("无法识别你的操作系统类型，请自己设置")
```


设置完相关参数以后，我们再来学习一下，如何每天教女友`学英语`
```python
# 获取每日励志精句
def get_message():
    r = requests.get("http://open.iciba.com/dsapi/")
    note = r.json()['note']
    content = r.json()['content']
    return note,content
```

只有每天的问候和节日问候是仅仅不够的，我们必须时刻知道她的情绪指数，这里可以使用snowNlp或者jieba来做分析，但是为了能够在打包成exe可执行文件时使得程序尽可能小，我们采取直接调用接口的方式来做。代码如下：
```python
# 接收女友消息监听器
# 女友微信名
my_girl_friend = bot.friends().search(my_lady_wechat_name)[0]
# chats=my_girl_friend 表示接收消息的对象，也就是女友
# except_self=False 表示同时也接收自己发的消息，不需要接收自己消息的可以去掉
@bot.register(chats=my_girl_friend, except_self=False)
def print_others(msg):
    # 输出聊天内容
    print(msg.text)

    # 做极其简单的情感分析
    # 结果仅供参考，请勿完全相信
    postData = {'data':msg.text}
    response = post('https://bosonnlp.com/analysis/sentiment?analysisType=',data=postData)
    data = response.text

    # 情感评分指数(越接近1表示心情越好，越接近0表示心情越差)
    now_mod_rank = (data.split(',')[0]).replace('[[','')
    print("来自女友的消息:%s\n当前情感得分:%s\n越接近1表示心情越好，越接近0表示心情越差，情感结果仅供参考，请勿完全相信！\n\n" % (msg.text, now_mod_rank))

    # 发送信息到文件传输助手
    mood_message = u"来自女友的消息:" + msg.text + "\n当前情感得分:" + now_mod_rank + "\n越接近1表示心情越好，越接近0表示心情越差，情感结果仅供参考，请勿完全相信！\n\n"
    bot.file_helper.send(mood_message)
```

教完女友`学英语`后，开始把我们的关心语发给他。这里涉及到wxpy模块的相关操作，很简单，看我的例子就会了。
```python
# 发送消息给她
def send_message(your_message):
    try:
        # 对方的微信名称
        my_girl_friend = bot.friends().search(my_lady_wechat_name)[0]

        # 发送消息给对方
        my_girl_friend.send(your_message)
    except:

        # 出问题时，发送信息到文件传输助手
        bot.file_helper.send(u"守护女友出问题了，赶紧去看看咋回事~")
```


最后，就是如何每天定时发关心语给女友的问题了。首先来个while循环，365天无限关心😀
```python
    # 来个死循环，24小时关心她
    while(True):

        # 提示
        print("守护中，时间:%s"% time.ctime())

        # 每天定时问候，早上起床，中午吃饭，晚上吃饭，晚上睡觉
        # 获取时间，只获取时和分，对应的位置为倒数第13位到倒数第8位
        now_time = time.ctime()[-13:-8]
        if (now_time == say_good_morning):
            # 随机取一句问候语
            message = choice(str_list_good_morning)

            # 是否加上随机表情
            if(flag_wx_emoj):
                message = message + choice(str_list_emoj)

            send_message(message)
            print("提醒女友早上起床:%s" % time.ctime())

		…………这下面还有很多代码，我就不列出来了…………

        # 延时60秒
        time.sleep(60)
```

最后，输入以下代码开始守护女友模式吧~
```python
    # 开始守护女友
    t = Thread(target=start_care, name='start_care')
    t.start()
```

## 使用教程

1. pip安装下列包
- [x] pip install wxpy
- [x] pip install requests

2. 设置以下内容
```python
[configuration]

# 设置女友的微信名称，记住，不是微信ID也不是微信备注
my_lady_wechat_name = 小强子


# 设置女友生日信息
# 若某一项月份或者日期不想设置，请输入99，不能留空
# 几月，注意补全数字，为两位数，比如6月必须写成06
birthday_month = 03
# 几号，注意补全数字，为两位数，比如6号必须写成08
birthday_day = 18


# 设置早上起床时间，中午吃饭时间，下午吃饭时间，晚上睡觉时间
# 若某一项时间不想设置，请输入99:00，不能留空
say_good_morning = 03:09
say_good_lunch = 03:10
say_good_dinner = 03:11
say_good_dream = 03:12


# 设置晚上睡觉问候语是否在原来的基础上再加上每日学英语精句
# 1表示是，0表示否
flag_learn_english = 1


# 设置所有问候语结束是否加上表情符号
# 1表示是，0表示否
flag_wx_emoj = 1


# 设置节日祝福语
# 情人节祝福语
str_Valentine = 亲爱的，情人节快乐！我想和你一起分享生命中的每一天，直到永远。

# 三八妇女节祝福语
str_Women = 嘿，女神节到了，祝我的女神开心快乐！你每天都是那么好看^_^

# 平安夜祝福语
str_Christmas_Eve = 宝贝，平安夜快乐，你吃苹果了吗？n(*≧▽≦*)n

# 圣诞节祝福语
str_Christmas = 圣诞节快乐哦！（づ￣3￣）づ╭❤～

# 她生日的时候的祝福语
str_birthday = 亲爱的，生日快乐，我已经给你准备好了礼物哦，明天你就能看到啦！(*@ο@*) 哇～
```
