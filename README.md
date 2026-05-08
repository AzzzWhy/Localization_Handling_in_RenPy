# Renpy Localization

## 关于Renpy本地化（翻译）的简陋系统

---

![alt text](<截屏2026-05-05 18.25.19.png>)

Ren'py, as an engine dedicated to visual novels, has consistently maintained a high level of recognition within the indie game development community for its ease of use. However, despite being such an outstanding game engine, there is a lack of in-depth exploration into its complete systems within the domestic community. In this repository, I will demonstrate how the Ren'py engine handles localization.

Renpy作为一个专注于视觉小说的引擎，其对于制作视觉小说的便利程度一直在独立游戏制作圈子里面认可度高，然而，就算是这样优秀的游戏引擎，在国内却缺乏对其完整系统的深挖。在这个仓库里面，我将会展示renpy引擎在本地化方面是如何完成的。

---

### First，the basic knowledge

![alt text](<截屏2026-05-05 18.36.17.png>)
When we open Ren'py, it is easy to observe that there is a 'Generate Translations' option near the bottom right of the page. Clicking into it will bring up the following window:

当我们打开renpy的时候，不难观察到，在靠近页面右下角的是有“Generate Translations”这个选项的，点击进去，会出现以下窗口：
![alt text](<截屏2026-05-05 18.39.36.png>)
Here, I will use English as a demonstration. After entering your desired language, you will find a new folder named 'English' created in this directory (\Renpy\save\test\game\tl), which contains the localization files we need

这里我拿英语作为示范，输入你想要的语言之后，你可以在这个目录下（\Renpy\save\test\game\tl），发现新建了一个名为English的文件夹，里面即是我们所需的本地化文件。

After confirming that this function can be executed, delete the folder, and let’s take a look at how to perform the language conversion (translation)

## 确认该功能可以被执行之后，删除该文件夹，我们来看该怎么进行语言转换（翻译）。

As we know, within the game root directory, there exist the following two files: screens.rpy and script.rpy. The former contains all the functions and the corresponding text displayed on buttons within the menus (such as the Preferences screen), while the latter contains all the dialogue/story text, including other narrative activities。

我们知道，在game这个根目录里面，存在着以下两个文件：screens.rpy,script.rpy。前者包含我们在菜单（prefs）里面所有的功能以及对应功能按钮上的文字，后者可以包含着所有的剧情文本，包括其他剧情活动。
![alt text](<截屏2026-05-05 18.52.03.png>)

What we are primarily focusing on is the screens.rpy file located in the game root directory. Let's double-click to open this file. Since we are building a simple localization system, we might as well add a new screen specifically for language switching (of course, you can also add this functionality to existing screens if you prefer).
Under the screen navigation section, we need to input the following code to create an independent page:

而我们主要关注的其实就是最开始在game根目录下的screens.rpy文件。我们双击打开该文件，既然要做一个简单的本地化系统，那么不妨直接添加一个界面，来作为语言切换界面（当然，你也可以在其他界面里面添加，随你心情）。
在“screen navigation”下，我们需要输入以下代码来新建一个独立的页面：
![alt text](<截屏2026-05-05 19.00.31.png>)

```python
textbutton _("语言") action ShowMenu("language_menu")
```

In this way, we have successfully created an independent language localization page.

这样，就可以得到一个独立的语言本地化页面了。

Next, we will arrange the toggle buttons for three languages (CN, EN, JP) on this new page. The code is as follows:

接着，我们在这个新页面上布置三门语言（CN，EN，JP）的切换按键，代码如下：

```python
screen language_menu():
    tag menu

    ## 使用系统通用的游戏菜单框架
    ## using the basic system
    use game_menu(_("语言选择 / Language")):

        vbox:
            xalign 0.5
            yalign 0.2
            spacing 40

            text _("Please select a language"):
                xalign 0.5
                size 40
                color "#ffffff"


            vpgrid:
                cols 2
                spacing 25
                xalign 0.5

                # 动作列表：[切换语言, 弹窗提示]
                # Windiws Reminder
                textbutton "简体中文" action [Language(None), Notify("语言已切换为中文")] style "lang_button"
                textbutton "English" action [Language("english"), Notify("Language set to English")] style "lang_button"
                textbutton "日本語" action [Language("japanese"), Notify("言語を日本語に設定しました")] style "lang_button"
            

style lang_button is button:
    xsize 380
    ysize 70
    background Frame(Solid("#44aa88"), 4, 4) # 绿色背景 Green
    padding (10, 10)

style lang_button_text:
    xalign 0.5
    yalign 0.5
    idle_color "#ffffff"
    hover_color "#ffe4b5"
    size 24

# 英文菜单 EN
screen menu_english:
    tag menu
    textbutton "Back":
        action ShowMenu("language_menu")
    textbutton "English":
        align (0.1, 0.1)
        action Notify("Language set to English", 2.0)
    text "English selected":
        xalign 0.5
        yalign 0.5
        color "#44aa88"

# 中文菜单 CN
screen menu_chinese:
    tag menu
    textbutton "Back":
        action ShowMenu("language_menu")
    textbutton "Chinese":
        align (0.1, 0.1)
        action Notify("语言设置为中文", 2.0)
    text "中文选中":
        xalign 0.5
        yalign 0.5
        color "#44aa88"

# 日文菜单 JP
screen menu_japanese:
    tag menu
    textbutton "Back":
        action ShowMenu("language_menu")
    textbutton "Japanese":
        align (0.1, 0.1)
        action Notify("言語を日本語に設定", 2.0)
    text "Japanese 選択":
        xalign 0.5
        yalign 0.5
        color "#44aa88"



screen language_menu():
    tag menu

    ## 使用系统标准框架
    ## using the basic system

    use game_menu(_("语言选择 / Language")):

        vbox:
            xalign 0.5
            yalign 0.2
            spacing 40

            text _("请选择语言"):
                xalign 0.5
                size 40
                color "#ffffff"

            ## 确保 vpgrid 能够接收点击
            ##State Monitoring
            vpgrid:
                cols 2
                spacing 25
                xalign 0.5
                mousewheel True
                draggable True

                # 动作列表
                # The list
                textbutton "简体中文" action [Language(None), Notify("语言已切换为中文")] style "lang_button"
                textbutton "English" action [Language("english"), Notify("Language set to English")] style "lang_button"
                textbutton "日本語" action [Language("japanese"), Notify("言語を日本語に設定しました")] style "lang_button"


style lang_button is button:
    xsize 380
    ysize 70
    background Solid("#08422f")
    hover_background Solid("#074430")            ##Of course select you favorite color
    selected_background Solid("#ffaa00")
    padding (10, 10)

style lang_button_text is button_text:
    xalign 0.5
    yalign 0.5
    idle_color "#2217b4"
    hover_color "#174106"
    size 24
```

With this, we now have three language toggle buttons. However, since we haven't yet set up the corresponding translation strings for each language, the buttons are currently non-functional. Don't worry, though—we will cover how to handle that in the next steps.

这样，我们就可以得到三个语言切换键了，只不过现在，我们并没有设置好对应语言的替换文字，所以按键暂时无效，不过不着急，之后就会说明如何操作。

---

Let’s return to the Ren'py main interface. Based on what I mentioned earlier, create one or two language folders. Once created, we will consolidate the files originally located in the game root directory into a single folder. Since I am using Chinese, I will organize them all into a folder named 'chinese'. Then, drag this folder into the following path (\Renpy\save\test\game\tl), placing it alongside the other one or two language folders

让我们回到renpy的主界面，根据我之前所说的，创建两个或者一个语言文件夹。创建好之后，我们把最开始在game的根目录上的这些文件整合到一个文件夹内，由于我使用的是中文，所以将她们全部整理到名为“chinese”的文件夹里。接着，将这个文件夹拖入这个路径（\Renpy\save\test\game\tl）下，与其他一个或者两个并列。
![alt text](<截屏2026-05-05 19.21.54.png>)
![alt text](<截屏2026-05-05 19.25.13.png>)

Open the other language folders (excluding 'chinese' in my case; follow your own setup), and click on screens.rpy. You will notice that we have already mapped the original language strings to the empty placeholders for the target language (similar to the image below). What you need to do next is use Google Translate or other translation tools to translate the text into the corresponding language and fill it in. This way, all the text in the menus will be replaced. For the story dialogue, simply edit the script.rpy file within the respective language folder; the system will automatically switch the content when the language is toggled."

打开除了“chinese”（我是中文，你们按照你们来）的其他文件夹，点击screens.rpy，你会发现，我们已经将语言与其要被替换的另一个另外一种语言的空闲位置所对应起来了（类似下图）。接下来你需要做的就是打开你的Google或者其他翻译软件，翻译到对应的语言并且填上去。这样，就可以替换菜单里面所有的文字了，文本剧情直接在对应语言文件夹之下的scripts.rpy编辑就行，语言切换的时候会自动切换。

## ![alt text](<截屏2026-05-05 19.33.21.png>)

That concludes the entirety of this document. Thank you for your patience in reading through it. As a beginner in programming, it is a tremendous honor for me to be able to assist you with your coding journey. If you find any errors or have any doubts, please feel free to send me an email to discuss them—let’s grow and improve together!

所以，这就是本文档的全部内容，感谢你有耐心能够看完这一篇文章，作为一名编程小白，能编程上帮到你是我莫大的荣幸。有任何不对或者存疑的地方，欢迎发送邮件来和我一起探讨，让我们一起成长！


AzzzWhy
