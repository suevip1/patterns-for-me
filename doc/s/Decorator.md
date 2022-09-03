## <center> 结构型 - 装饰器（Decorator）设计模式
---

本篇我们将介绍装饰器模式，如果你想快速记忆，那么只需要抓住装饰器模式的特征：它允许多个对象可以像俄罗斯套娃一样层层嵌套。

> 你甚至可以把装饰器模式想象为项目转包，`甲`与`乙`签订了项目合同，`乙`转头又将该项目转包给`丙`，而`丙`同样可以将项目交给外包团队`丁`来实施。`丁`作为项目的真实实施者，最终得到了项目合同金额的 25%。其余的利润被转包者瓜分，`丙`抽取 35%，`乙`抽取 40%。项目的层层转包及抽取佣金的过程，就可以用装饰器模式来描述。

# 一、问题引入

假如你和你的团队已经开发了一款线上购物平台，商家在平台上发布商品，用户可以浏览感兴趣的商品并进行购买。尽管你们在货源上已经尽量压低商品价格，并且平台并未收取任何的服务费，但销售额始终不太理想。经过公司高层的商议，决定采取产品组同事推出的新方案：以优惠来刺激消费者。最终，你收到产品组同事提出的如下商品优惠方案。

> - **满减券**：比如满 100 减 20，如果金额没有达到门槛额度，则不能使用该满减券；
> - **抵用券**：直接抵用，当金额低于抵用券金额时，以金额值为抵用额度；
> - **折扣券**：在原始价格基础上进行折扣；
> 
> 以上三种优惠券可相互叠加使用，并且同类型的优惠券也可叠加。除此之外，需求中还要求了用户在购物车内浏览商品时，可计算出选中商品的原始总价、优惠后价格和优惠的计算依据。

我们该如何描述购物车内商品所使用的优惠呢？或许可以按照优惠类型进行分类，并为其提供特定的优惠计算器对象表示。比如我们可以将优惠分为使用`满减计算器`、`抵用计算器`和`折扣计算器`。但同时，我们也要考虑多种优惠的组合，由此衍生出来有：`满减&抵用计算器`、`满减&折扣计算器`、`抵用&折扣计算器`、`满减&抵用&折扣计算器`。
<div align="center">
   <img src="/doc/resource/decorator/优惠类型扩展示意图.png" width="60%"/>
</div>

此时，我们似乎意识到一些如下的问题。

**（问题一）如何描述同一种优惠类型的叠加**

对于同类型优惠券的叠加，上述的设计无法解决。比如，商品使用了两张抵用券和一张满减券的优惠组合，我们无法借助上述的任何一种优惠计算器来表述。

**（问题二）扩展将带来类爆炸**

到目前为止，我们仅有 3 种优惠类型，采用上面的方式来表述所有的可能，我们需要定义 7 个类（3 一种优惠 + 3 两种优惠组合 + 1 三种优惠组合）。但如果此时需要新增一种优惠类型，那么就需要【4 （一种优惠）+ 6（两种优惠组合）+ 4（三种优惠组合）+ 1（四种优惠组合）】15 个类进行表示。随着优惠类型的扩展，系统中类的数量将呈现几何式的上升，这是典型的类爆炸。

**（问题三）优惠的顺序很重要**

对于组合优惠来说，优惠顺序很重要。假如现有满减&折扣类型的商品，商品总价为 100 元，满减券为满 50 减 20，折扣券为七折。

- 先使用满减券：实际价格 = (100 - 20) * 0.7 = 56
- 先使用折扣券：实际价格 = (100 * 0.7) - 20 = 50

对于同样的优惠组合来说，不同的使用顺序将有不一样的最终价格。在这个例子中，优惠的使用顺序很重要。而上面的案例则无法表示同样优惠组合的不同使用顺序。

# 二、解决方案

## 2.1 组合代替继承
在前面我们尝试使用继承来扩展类，但在解决这个问题时明显行不通。或许我们一开始就陷入了错误的方向，我们无法表示所有商品使用的优惠类型，因为我们无法穷尽所有的优惠类型的可能。在这个问题的解决方案上面，我们不得不另辟蹊径。如果你已经在面向对象的圈子中混迹过一段时日，那么你应该听过一句广为流传的名言：组合优于继承。是的，采用组合代替继承，能完美解决继承所带来的类爆炸问题。

## 2.2 递归向下求解
现在，我们仅剩下问题一和问题三。回想一下，在上面我们是如何计算购物车内商品的最终价格的？

| 步骤 | 优惠 | 计算过程 | 最终价格（元） |
| --- | --- | --- | --- |
| 1 | 商品总价 | / | 200.00 |
| 2 | 满100减20 | 200.00 - 20 | 180.00 |
| 3 | 8.5折 | 180.00 * 0.85 | 153.00 |
| 4 | 50元抵用券 | 153.00 - 50 | 103.00 |
| 5 | 20元抵用券 | 103.00 - 20 | 83.00 |

正如我们在问题中描述的那样，优惠的先后顺序可能影响最终的价格，所以价格计算的步骤是预定义且不允许修改的，否则我们将得到错误的最终结果。而对于任意一个组合优惠来说，最终的价格就是优惠的顺序叠加。如上表所示，一个优惠组合的价格计算可以总结为：从当前的最终价格中扣除即将优惠的价格，而当前的最终价格等于前一个优惠的最终价格扣钱前一个优惠的价格。这样说可能有些绕，或许下面的图更有助于理解。
<div align="center">
   <img src="/doc/resource/decorator/商品价格计算过程.png" width="80%"/>
</div>

正如该计算过程图所示，商品的最终价格从商品的原始总价一步一步的计算得出。如果我们有办法将每一次优惠的价格计算都封装在对象内部处理，并且各个对象可以相互替换，那剩下的两个问题也都能迎刃而解。

> 我们可以动态的为现有的优惠组合插入新的优惠类型，而不用关心该优惠的具体类型，尽管组合中已经有该优惠的类型了，问题一得到解决。优惠的顺序就是处理链的顺序，按照特定方式的构建，必将按照同样的方式进行计算，问题三也不复存在。

而上面的要求正是装饰器模式所提倡的，装饰器模式建议我们将复杂问题的求解过程拆分开来，并且在对象直接进行层层委托，直到该对象已经无法再委托给任何其他对象为止。并且，装饰器模式要求所有的对象都具有同样的行为定义，这样就可以轻松的在运行时对所有对象进行任意的排列组合。

# 三、案例实现
按照解决方案中的分析，我们来逐步实现该案例。在前面我们说道：装饰器模式要求所有的对象都必须具有同样的行为定义，所以，我们定义一个抽象的费用计算器`CostCalculator`，案例中的所有对象（各种优惠类型计算器、购物车）都必须实现费用计算器的行为。

除此之外，我们注意到购物车本身和其他优惠类型有些细微差别，购物车本身不具备任何优惠行为，购物车的费用计算就是各种商品列表的原价总和。而各个优惠计算器对象必须依赖于购物车或者其他的优惠计算器对象，因为他们的原始费用来源于所依赖的对象。总而言之，所有的优惠计算器对象必须依赖一个已有的费用计算器对象。

## 3.1 案例类图
按照上诉的分析，我们得到如下的类图结构。
<div align="center">
   <img src="/doc/resource/decorator/案例类图.png" width="90%"/>
</div>

在该类图中，`CostCalculator`表示抽象的费用计算器，定义了两个行为，分别是优惠的描述`description()`和计算费用`finalCost()`。`ShoppingCart`代表购物车，除了实现`CostCalculator`中定义的行为外，还有添加商品`addGoods()`和获取商品列表的详细信息`getDetails()`，`GoodDetail`为商品类。`AbstractCostDecorator`为抽象的优惠计算器，统一定义了所有优惠计算器所需的依赖`calculator`，三个实现类分别是折扣计算器`DiscountDecorator`、满减计算器`FullDiscountDecorator`和抵用计算器`VoucherDecorator`。在满减计算器中，提供了获取当前总金额是否跨过满减门槛的方法`aboveThreshold()`。

## 3.2 代码附录

**（1）费用计算器**
```java
public interface CostCalculator {

    /**
     * 计算描述
     * @return 描述
     */
    String description();

    /**
     * 商品的最终费用
     * @return 最终费用
     */
    BigDecimal finalCost();

}
```
**（2）购物车**
```java
public class ShoppingCart implements CostCalculator {

    private final List<GoodsDetail> goods = new ArrayList<>();       // 商品清单

    public void addGoods(GoodsDetail detail) {
        this.goods.add(detail);
    }

    @Override
    public String description() {
        return "商品总费用";
    }

    @Override
    public BigDecimal finalCost() {
        BigDecimal totalCost = BigDecimal.ZERO;
        for (GoodsDetail item : goods) {
            BigDecimal cost = item.price.multiply(BigDecimal.valueOf(item.number));
            totalCost = totalCost.add(cost);
        }
        return totalCost;
    }

    /**
     * 获取商品明细
     * @return 商品明细
     */
    public String getDetails() {
        List<String> details = goods.stream()
                .map(o -> MessageFormat.format("      商品名：【{0}】，商品单价：【{1}元】，商品数量：【{2}】",
                        o.name, o.price, o.number))
                .collect(Collectors.toList());
        return String.join("\n", details);
    }


    /**
     * 商品明细
     */
    public static class GoodsDetail {
        private final String name;            // 名称
        private final BigDecimal price;       // 单价
        private final int number;             // 数量
        public GoodsDetail(String name, BigDecimal price, int number) {
            this.name = name;
            this.price = price;
            this.number = number;
        }
    }
}
```
**（3）优惠计算器**

**（3-1）抽象的优惠计算器**
```java
public abstract class AbstractCostDecorator implements CostCalculator {

    /**
     * 包装的费用计算器
     */
    protected final CostCalculator calculator;

    public AbstractCostDecorator(CostCalculator calculator) {
        this.calculator = calculator;
    }
}

```
**（3-2）折扣券费用计算器**
```java
public class DiscountDecorator extends AbstractCostDecorator {

    /**
     * 折扣
     */
    private final BigDecimal discount;

    public DiscountDecorator(CostCalculator calculator, BigDecimal discount) {
        super(calculator);
        this.discount = discount;
    }

    @Override
    public String description() {
        return calculator.description() +
                MessageFormat.format(" -> {0}折扣", discount.multiply(BigDecimal.valueOf(10)));
    }

    @Override
    public BigDecimal finalCost() {
        // 费用 * 折扣
        return calculator.finalCost().multiply(discount);
    }
}
```
**（3-3）满减券费用计算器**
```java
public class FullDiscountDecorator extends AbstractCostDecorator {

    /**
     * 满减券面额
     */
    private final BigDecimal amount;

    /**
     * 满减门槛
     */
    private final BigDecimal threshold;

    public FullDiscountDecorator(CostCalculator calculator, BigDecimal amount, BigDecimal threshold) {
        super(calculator);
        this.amount = amount;
        this.threshold = threshold;
    }

    @Override
    public String description() {
        if (aboveThreshold()) {
            return calculator.description() +
                    MessageFormat.format(" -> 满{0}减{1}", threshold, amount);
        }
        return null;
    }

    @Override
    public BigDecimal finalCost() {
        if (aboveThreshold()) {
            // 费用 - 减去优惠面额
            return calculator.finalCost().subtract(amount);
        }
        // 无法满减 -> 原价
        return calculator.finalCost();
    }

    /**
     * 原始金额是否高于满减门槛
     * @return true:是
     */
    public boolean aboveThreshold () {
        return calculator.finalCost().compareTo(threshold) > 0;
    }
}
```
**（3-4）抵用券费用计算器**
```java
public class VoucherDecorator extends AbstractCostDecorator {

    /**
     * 抵用券面额
     */
    private final BigDecimal amount;

    public VoucherDecorator(CostCalculator calculator, BigDecimal amount) {
        super(calculator);
        this.amount = amount;
    }

    @Override
    public String description() {
        return calculator.description() +
                MessageFormat.format(" -> {0}元抵用券", amount);
    }

    @Override
    public BigDecimal finalCost() {
        // 费用 - 减去抵用券面额
        return calculator.finalCost().subtract(amount);
    }
}
```
**（4）客户端**

**（4-1）Client**
```java
public class Client {
    public static void main(String[] args) {
        System.out.println("|==> Start --------------------------------------------------------------|");
        ShoppingCart cart = new ShoppingCart();
        // 添加商品
        cart.addGoods(new ShoppingCart.GoodsDetail("夏季T恤", BigDecimal.valueOf(59.9), 2));
        cart.addGoods(new ShoppingCart.GoodsDetail("网球拍", BigDecimal.valueOf(100), 1));
        cart.addGoods(new ShoppingCart.GoodsDetail("网红款家用驱蚊液", BigDecimal.valueOf(28.5), 2));
        System.out.println(MessageFormat.format("   购物车商品明细：\n{0}", cart.getDetails()));
        System.out.println(MessageFormat.format("   商品原价：【{0}元】", cart.finalCost()));

        // 添加优惠：一张折扣券（8.5折）、一张满减券（满100减20）、两张抵用券（20元、5元）
        VoucherDecorator decorator =
                new VoucherDecorator(
                        new VoucherDecorator(
                                new FullDiscountDecorator(
                                        new DiscountDecorator(cart, BigDecimal.valueOf(0.85)), BigDecimal.valueOf(20), BigDecimal.valueOf(100)
                                ), BigDecimal.valueOf(20)
                        ), BigDecimal.valueOf(5)
                );
        System.out.println(MessageFormat.format("   优惠后价格：【{0}元】，优惠说明：【{1}】",
                decorator.finalCost(),
                decorator.description()));
    }
}
```
**（4-2）运行结果**
```text
|==> Start --------------------------------------------------------------|
   购物车商品明细：
      商品名：【夏季T恤】，商品单价：【59.9元】，商品数量：【2】
      商品名：【网球拍】，商品单价：【100元】，商品数量：【1】
      商品名：【网红款家用驱蚊液】，商品单价：【28.5元】，商品数量：【2】
   商品原价：【276.8元】
   优惠后价格：【190.28元】，优惠说明：【商品总费用 -> 8.5折扣 -> 满100减20 -> 20元抵用券 -> 5元抵用券】
```

# 四、装饰器模式
## 4.1 意图
> **指在不改变原有对象结构的基础情况下，动态地给该对象增加一些额外功能的职责。**

装饰器模式的核心就是在运行时，不停的给一个对象添加一些功能。操作手法就是把一个包装好的对象作为原始对象再次进行包装。

> 想象一下我们煮了一碗面条，往面条里面放入一勺牛肉，这样就变成了牛肉面；再往面条里面放入一勺酸菜，这样就变成了酸菜牛肉面。但不管往面条里面加入多少的配菜，主食必不可少。这也很像中文里面的形容词和名词的关系，名词可以被多个形容词进行修饰。

## 4.2 类图结构
让我们看一下更加通用的装饰器模式的类图结构：
<div align="center">
   <img src="/doc/resource/decorator/经典装饰器模式类图.jpg" width="50%"/>
</div>

装饰器模式的参与角色如下：

- **Component**：组件，定义一个对象接口；
- **ConcreteComponent**：具体的组件，无法包装组件对象的原始组件；
- **Decorator**：抽象的装饰器，本身是组件的实现，同时也包装了一个组件；
- **ConcreteDecorator**：具体的装饰器实现；

# 五、深入
## 5.1 使用技巧

**（1）省略抽象的装饰器**

当你仅需一个ConcreteDecorator时，没有必要定义抽象的Decorator。

**（2）尽量保证Component的简单性**

在装饰器模式中，高层的组件（Component）是所有对象的根源，因此应该尽量保证 Component 的简单性。否则，Component 难以被大量复用，越简单的定义复用性的可能性越高。建议仅仅在 Component 中定义接口，而不要存储任何数据。如果现有的 Component 已经相当庞大了，此时使用装饰器模式可能为此付出的代价过高，可以考虑使用[策略模式（Strategy）](https://www.yuque.com/coderran/pd/mgbgzd)而不是装饰器模式。

# 六、在源码中看装饰器模式
我们在各种源码中都能看到装饰器模式的影子，例如：

**（1）java.io.InputStream**

<div align="center">
   <img src="/doc/resource/decorator/InputStream.png" width="70%"/>
</div>

**（2）javax.servlet.ServletRequestWrapper**

<div align="center">
   <img src="/doc/resource/decorator/ServletRequestWrapper.png" width="80%"/>
</div>

ServletRequestWrapper是ServletRequest接口的装饰器实现，开发者可以继承ServletRequestWrapper去扩展原有的ServletRequest。


# 附录
[回到主页](/README.md)    [案例代码](/src/main/java/com/aoligei/structural/decorator)