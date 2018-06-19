- [lifecycle](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
- [http://juvenshun.iteye.com/blog/213959](http://juvenshun.iteye.com/blog/213959)
- [http://blog.csdn.net/benjamin_whx/article/details/44944897](http://blog.csdn.net/benjamin_whx/article/details/44944897)
- [http://www.yiibai.com/maven/](http://www.yiibai.com/maven/)

# 生命周期 #

整体结构是生命周期(lifecycle),阶段(phase),目标(goal)，常用方式是调用阶段，比如mvn clean

maven分为3个生命周期，这三个是独立的，各自都是完整的整体

- Clean Lifecycle	看名字
- Default Lifecycle	编译测试打包部署之类的
- Site Lifecycle	站点相关工作

# 具体周期 #

## Clean Lifecycle ##
1. pre-clean 
2. clean
3. post-clean

mvn clean执行的就是这里的clean阶段。

执行某个阶段会把之前的阶段一起执行，比如说mvn clean会执行1和2，mvn post-clean会执行123(这里需要自己试下，特别是在idea中)

