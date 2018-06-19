一阶谓词逻辑与概率图模型的结合，类似下图关系

![](http://i.imgur.com/RSNJ4lF.png)

sat能解决紧的asp程序，同样mln能解决紧的lpmln程序

- 或许需要看下sat和mln
	- sat和smt感觉很相似？
	- mln是规则加权的一阶谓词逻辑库

A Markov network (also known as Markov random field)？？？？？？

这里是马尔科夫网，没有逻辑，概率图模型还是要计算一系列变量取值的概率

![](http://i.imgur.com/dPfdlsI.png)

x{k}是指第k个小圈子的状态，clique，个人理解应该是内部强耦合外部无关联的变量集合，φk是概率函数，整体表示取目标值的概率，另，这个概率常用对数线性模型，这么一说怎么觉得这个模型是用来机器学习的......小圈子代表加的先验？

一阶谓词逻辑->常量、变量、函数、谓词

Constant symbols represent objects in the domain of interest (e.g., people: Anna, Bob, Chris, etc.).

Variable symbols range over the objects in the domain.

Function symbols (e.g., MotherOf) represent mappings from tuples of objects to objects.

Predicate symbols represent relations among objects in the domain (e.g., Friends) or attributes of objects (e.g., Smokes).

**函数强调映射，而谓词强调的是参数之间满足某个关系**

这里可以看出常量、变量、函数是在同一等级上的，谓词与之不同

term项就是常量、变量、实例化的函数

原子公式就是一个直接使用项实例化的谓词

一个知识库中公式都为真，所以可以看作一个连接起来的大公式

从mln的定义来看，mln和lpmln十分类似，试着找找区别

Free (unquantified) variables？


知识库合并之前理解无误

- 或许需要看下problog

稳定模型语义与之前理解一致，集合蕴含意义上的最小正确回答
 
另：A logic program is called **ground** if it contains no variables.还有grounding的时候是没有函数常量的（也就是说被替换的地方没有函数？感觉挺对），但按文中说是ground的时候替换，这么说函数常量还是会出现，在哪些不被替换的地方，（比如这个？a:=b(f)，不知道可以不）

lpmln程序：带权规则集{w：R}R是规则，w=R∪{α}R是实数（负的？）

Instead, each stable model is obtained from some **subset** of the program, and the weights of the rules in that subset determine the probability of the stable model.天然划分？

已有稳定模型I和程序pie，像pie中添加规则，如果I满足新的规则，则其还是稳定模型，不满足则不是

![](http://i.imgur.com/RSRm2Ct.png)

SM[pie]是稳定模型集合,但不是pie上的稳定模型，I是稳定模型，计算方式与之前理解一致，正则化



根据强规则如果可以推出稳定模型，则根据所有规则推出的稳定模型必然满足所有强规则，因为α全都无穷了，不满足所有强规则的回答集的权重都是0。满足所有强规则同时表明包含强规则的某个稳定模型。

The idea of softening rules in LPMLN is similar to the idea of weak constraints in ASP.

- asp弱约束得看下
- #P-hard，#P-complete
- Gibbs sampling

