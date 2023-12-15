# 对扁平化的简单理解


Python 之禅，第五条：
> Flat is better than nested.  

翻译成中文，**扁平优于嵌套**。无论在生活，软件开发，还是经营管理中都适用。

## 先从简单判断中理解
根据变量`x`的值，计算变量`y`的值。

### 嵌套写法
```python
if x > 1:
    y = 3 * x - 5
else:
    if x >= -1:
        y = x + 2
    else:
        y = 5 * x + 3
```

### 扁平写法
```python
if x > 1:
    y = 3 * x - 5
elif x >= -1:
    y = x + 2
else:
    y = 5 * x + 3
```

假设新增一个条件，当`x`小于等于-10 时，`y`的值等于 `x * 2`。

### 嵌套写法
```python
if x > 1:
    y = 3 * x - 5
else:
    if x >= -1:
        y = x + 2
    else:
        if x <= -10:
            y = x * 2
        else:
            y = 5 * x + 3
```

### 扁平写法
```python
if x > 1:
    y = 3 * x - 5
elif x >= -1:
    y = x + 2
elif x >= -10:
    y = 5 * x + 3
else:
    y = x * 2
```

新增条件之后，代码对比，扁平化的优势就体现出来了。
1. 可读性更强，扁平化只需要新增`elif`分支。代码层次结构更加清晰。（ps. 较短的嵌套，感觉不强烈，当出现 4 层嵌套以上，扁平化的优势就体现出来了）
2. 代码更加简洁，不需要增加额外的嵌套代码。
3. 易于维护。

## 扁平化在代码重构中的实践
平常在编写代码过程中，经常会出现下面这种代码，经过扁平化优化后，看看代码会有什么样的效果。

### 优化前
```python
def process_data(data: dict[str, Any] | None) -> bool:
    if data is not None:
        if "value" in data:
            value = data["value"]
            if value > 10:
                return True
            else:
                print("Value is too small")
                return False
        else:
            print("No value in data")
            return False
    else:
        print("No data provided")
        return False
```

1. Inverting conditions（guard clauses 保护子句）
2. Bailing out early（尽早返回）
3. Doing more of these refactorings （做更多类似的优化）
4. Making it even shorter （变得更简短）

### 优化后
```python
def refactor_process_data(data: dict[str, Any] | None) -> bool:
    if data is None:
        print("No data provided")
        return False

    if "value" not in data:
        print("No value in data")
        return False

    return data["value"] > 10
```

优化过后，代码更简洁，可读性更强。

### 组织中的扁平化
传统企业中，组织架构图，如同金字塔结构，等级分明，一层压层，流程繁琐，不论高层发号施令，还是底层向高层意见反馈，都会不能及时传达，执行力降低，导致效率低下，由于中间管理层过多，人员臃肿，企业成本大大增加。种种原因，最终导致企业竞争力下降，最后关门倒闭。

![金字塔结构](https://image.ericzzz.com/2023/12/15/7c0ace9d-525b-44aa-9b9e-d4ec134b0ffa.png)

扁平化组织

- 由于管理层比较少，沟通渠道更短，更直接。促进整个组织更好更快的沟通，促进想法，信息和反馈的交流

- 高层管理者对组织运营有更清晰的了解，并且可以做出明智的决策，而无需穿过多个管理层。

- 扁平化结构会给予员工更多的自主权和决策权。这可以带来更高的工作满意度，动力和工作主人翁感。

- 较少的等级制度，可以减少内部政治和权力的斗争。

- 更加灵活，快速响应市场需求。不受官僚主义的毒害。

![扁平化结构](https://image.ericzzz.com/2023/12/15/406e23d5-0d5f-4639-88e8-8bd90140ace6.png)

