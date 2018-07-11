---
title: 安全的重叠构造器 - 最佳实践：Build Pattern
keywords: [java]
date: 2018/1/28 0:20
tags: [java]
categories: java
---
```
effective java # 2
```
Builder模式，不直接生成想要的对象，而是让客户端利用所有必要的参数调用构造器，

得到一个builder对象。然后客户端在builder对象上调用类似于setter的方法，

来设置每个相关可选的参数，最后调用无参的build来生成不可变的对象。

完整代码+测试：<a href="https://github.com/hisenyuan/IDEAPractice/tree/master/src/main/java/com/hisen/interview/effective/no2buildpatter" target="_blank">github:完整代码+测试</a>
```
public class NutritionFacts {

  private final int servingSize;
  private final int servings;
  private final int calories;
  private final int fat;
  private final int sodium;
  private final int carbohydrate;

  private NutritionFacts(Builder builder) {
    servingSize = builder.servingSize;
    servings = builder.servings;
    calories = builder.calories;
    fat = builder.fat;
    sodium = builder.sodium;
    carbohydrate = builder.carbohydrate;
  }

  public static class Builder {

    // Required parameters
    private final int servingSize;
    private final int servings;

    private int calories = 0;
    private int fat = 0;
    private int carbohydrate = 0;
    private int sodium = 0;

    public Builder(int servingSize, int servings) {
      this.servingSize = servingSize;
      this.servings = servings;
    }

    public Builder calories(int val) {
      calories = val;
      return this;
    }

    public Builder fat(int val) {
      this.fat = val;
      return this;
    }

    public Builder carbohydrate(int val) {
      this.carbohydrate = val;
      return this;
    }

    public Builder sodium(int val) {
      this.sodium = val;
      return this;
    }

    public NutritionFacts build() {
      return new NutritionFacts(this);
    }
  }



  @Override
  public String toString() {
    return "NutritionFacts{" +
        "servingSize=" + servingSize +
        ", servings=" + servings +
        ", calories=" + calories +
        ", fat=" + fat +
        ", sodium=" + sodium +
        ", carbohydrate=" + carbohydrate +
        '}';
  }
}
```