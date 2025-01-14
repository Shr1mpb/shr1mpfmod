*更新时间： 2025.1.11*

## 介绍

这里是Shr1mp的MineCraft Fabric模组开发学习

如果你看完了这篇README，那么你也可以入门Minecraft的Fabric模组开发(1.21.2+)的简单功能，我相信这些功能足以帮你实现类似 Bilibili 上的 "你说我做" 系列节目。方法就是观看这位博主在Youtube上的视频(因为那个视频只适用于1.21.2-,很多方法都无法正常使用了)，并来我这里查看我封装好的方法。或者你也可以自己封装自己的方法，或者使用原生的Fabric API。
我个人还是感觉把原生的方法都封装一下，编程体验会大大提高，也会巩固一些编程语言知识，提高代码水平。

**这里我用的版本为Java 1.21.4**

在这里你可以**套用我封装好的各种方法**(例如创建物品、方块和物品栏等)。

你可以直接去对应的类中寻找(**推荐**)。

_如果你需要帮助，我的邮箱是1205874457@qq.com(常用) / chnrzh2004@gmail.com(不常用)_ 

---

## 初衷：

* 看到网上很多模组教学都是之前的，1.21.4的Fabric API发生了较大的变化，之前的注册物品和方块的功能可能都需要被改进，即便是在Fabric Wiki上也有没有的信息，需要自己挖掘。
* 我这边跟着Youtube博主[Modding by Kaupenjoe](https://www.youtube.com/@ModdingByKaupenjoe) 学习，并**自己封装**了很多新版本的Fabric API的方法，让我能**快速的注册物品、注册方块或修改对应的属性等等。**
* 我的仓库这几天会持续更新，跟着旧版本的教学走一次，如果无法成功，我会找解决方法，之后封装好我的函数。**每个类或函数的用法我已经写在了对应的java类中。**


## Tips/食用方法/简易教程：

无论如何，我都建议你去查看这位博主的视频[Modding by Kaupenjoe](https://www.youtube.com/@ModdingByKaupenjoe)，有初中的英文水平就能看懂，他的视频长达11小时，涵盖面广，教学详细。

在遇到不生效的情况时，可以来这里查找解决方案，或者使用我封装好的方法。你也可以去Fabric开发者的社区、论坛或是[Fabric Wiki](https://wiki.fabricmc.net/start)寻找解决方法。

开发指引/本仓库使用方法 
---
* **多使用External Libraries。**
对于`resource/data/【modid】/recipe`中的配方，包括合成、冶炼等，你可以在`External Libraries`中找到原版Minecraft中的一些配方，仿照他们，就能得到很不错的结果。例如对于九个Slot都一样的配方(例如合成xxx块)，在新版本中和旧版本不一样，需要仔细甄别。我也强烈建议你多使用这种方法查找解决方法，或者达成自己想要的目标(当然是你得对原版物品的机制熟悉，haha)


* `resource/assets/【modid】`中是添加**材质和模型**的地方。就Item举例而言，三层结构：items -> models -> textures，仿照书写即可


* **提交记录**中的Init就是有对API进行封装的提交，Perf或者Fix就是对这些东西的改进或修复


* 要让创建的方块被**破坏后有掉落物**或**设置开采等级**，至少需要两步。首先需要在`resource/data/tags/block/mineable`的对应的工具中添加相应的方块，如果要设置开采等级，需要在上一级的needs_XXX_tool中添加；然后，在`resource/data/【modid】/loot_table/blocks`中添加json文件定义其掉落的物品(掉落自己本身的方块的json文件都长得差不多，参照此文件夹中的first_block.json即可；对于矿石类型的方块，还有附魔、掉落物配置之类，相对复杂，参考second_block.json)。


* **自定义物品(创造超脱原版的特定功能的新物品)**：参照`src/main/java/com/shr1mp4zh/fmod/item/custom/ChiselItem`类，在这个类上有一定的注释说明;然后，去到ModItems里用我封装的`registerCustomItem()`方法注册这个物品。原理是重写原版的Item类，它可以帮助你实现很多新的功能，或者实现你从小就想实现的梦想。
 Item类也有很多子类，有原版中实现各种各样功能的类供你参考。选中Item，使用Ctrl+H即可查看(IDEA中)。


* **自定义方块**：_与自定义物品相似_，参照`src/main/java/com/shr1mp4zh/fmod/block/custom/MagicBlock`，然后再将它通过`registerCustomBlock()`方法注册到游戏中。


* **自定义食物**：参考`src/main/java/com/shr1mpfzh/fmod/item/ModFoodComponents`中我封装的方法与创建的`CAULIFLOWER`属性，创建完成之后要去ModItems中使用`registerCustomFood()`注册即可，注册时留空药效相关的属性。

* **自定义食物药效**：注意：食物添加药水效果的方法与1.21.2-有所不同，需要先创建普通的FoodComponent，然后去同目录下的`ModConsumableComponents`中创建药水效果，最后在ModItems注册时加入药水效果，这里已经封装到了ModItems的`registerCustomFood()`方法中。可以参考CAULIFLOWER的创建。


* **注册燃料/堆肥物品**：注册一个新的物品后，在`src/main/java/com/shr1mpfzh/fmod/item/ModItems`下的`registerFuels()`方法的lambda表达式中添加即可;堆肥物品类似，在`registerCompostingChance()`方法中。


* **设置物品发光和物品稀有度**：在`createDefaultItemSettings()`方法后使用`.component(DataComponentTypes.ENCHANTMENT_GLINT_OVERRIDE, true)`;同理，使用`.rarity(Rarity.XXX)`可以设置物品稀有度(会影响物品名字的颜色)。


* **设置物品提示**：参考`src/main/java/com/shr1mp4zh/fmod/block/custom/MagicBlock`中的`appendToolTip()`。方法就是写一个物品类的子类，并且重写其appendToolTip()方法。此外，还可以添加`if(Screen.hasShiftDown())`判断来动态添加提示内容。


* **关于自定义标签**：这里没有做自定义标签的相关内容。自定义标签的作用是，能把特定的物品配置到 json 文件中，然后在代码中用一行就可以从文件中检索，比起配置到java类中要清爽很多。感兴趣可以看一下原视频的 [第10集 CustomTags](https://www.youtube.com/watch?v=lVpV3B3yFsg) 。




---

下面是一些我的废话：

> 我在2013年左右接触了Minecraft，那会刚上小学没多久，被这个神奇的世界吸引。之后我小学包括初中的绝大部分时间都在这款游戏上度过，只不过那会玩的都不是正版，都是国内的一些盗版服务器。
> 
> 之后我也有一段自己开服的经历，这个游戏可谓是我除了Counter-Strike(可能比它还长)游玩时间最长的游戏。
> 
> 上了大学后，当我意识到我认识它已经过去了12年，我才知道我没有自己一个人通关过任何一次这款游戏。于是我在2025.1.3~1.5花了两天时间，纯净无作弊单人极限生存通关了这款游戏。
> 
> 之前我认为这样的游戏是无聊的。因为我总是和朋友一起联机，总是会加入很多辅助性的、大大降低游戏难度的MOD。在我通关一次后，我明白了这个游戏也是会有很多乐趣的。沙盒游戏的开放程度很高，自由度也很高，如果我能按自己的想法创造游戏机制、游戏内容，那该有多幸福！
> 
> MC社区能发展到今天，离不开MOD的帮助。我小时候也有自己学习写MOD的想法，只不过那会什么都不会。现在刚刚学习了JavaWeb的一套内容，Java语言也用得相对来说比较熟练了，突发奇想，就进入了模组的学习。
> 
> 希望自己能坚持下去吧。既然做了，就做出成果，做出可以供游玩的MOD出来。共勉。 于2025.1.11
