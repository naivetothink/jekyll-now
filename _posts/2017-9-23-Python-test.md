---
layout: post
title: python列表相关操作（1）
---


直接贴代码：
```python
# 开始
# 定义一个列表
colors = ['blue', 'red', 'yellow']
# 打印列表中的第一个和第三个
print(colors[0])
print(colors[2])
# 打印列表中的最后一个
print(colors[-1])
# 取值并拼接,title()的作用是首字母转化为大写
message = 'I like '+colors[0].title()+'.'
print(message)
# 修改元素
print(colors)
colors[0] = 'green'
print(colors)
# 添加元素至末尾
colors.append('black')
print(colors)

# 插入元素insert(index,object),列表中其他元素右移。
colors.insert(0, 'white')
print(colors)
# 删除元素
del colors[1]
print(colors)

# pop() 弹出栈 弹出最后一个
last_color = colors.pop()
print('last_color: '+last_color)
print(colors)
# pop(index) 弹出栈 弹出下标为index的元素
first_color = colors.pop(0)
print('first_color: '+first_color)
print(colors)
# 删除值 remove('name'),没有返回值，要想使用删除的值需赋给变量
too_light = 'red'
colors.remove(too_light)
print('too_light: '+too_light)
print(colors)

colors = ['yellow', 'blue', 'red']
# sort() 永久性排序
print(colors)
colors.sort()
print(colors)
# sort(reverse=True) 与字母相反顺序排列
colors.sort(reverse=True)
print(colors)
# 临时性排序
print(sorted(colors, reverse=False))
# reverse()永久反转列表元素排列顺序，再次调用恢复原先排列
nums = [2, 5, 3, 6, 9]
print(nums)
nums.reverse()
print(nums)
nums.reverse()
print('再次调用 ')
print(nums)
# len(obj)确定元素长度
leng = len(nums)
# str(obj),转化为字符串
print('列表长度'+str(leng))
# for循环 许加’：‘，循环语句需缩进
for num in nums:
    print(num)
    # 循环体内
    print('a number')
# 循环体外
print("all number")
print('临时变量依然存在，为最后一个值为： '+str(num))
# 使用函数range('first num','last num'),结果first---(last-1)
for value in range(1, 9):
    print(value)
print('NO 9 :<')
# 使用list()将生成的数字转化为列表
print(list(range(1, 12)))
# range(start，end，step)指定步长
print(list(range(1, 12, 3)))

# 值得平方运算 range(2, 100000001)会死机！！！
squares = []
for value in range(2, 11):
    squares.append(value**2)
print(squares)
# min() max() sum()
print(min(squares))
print(max(squares))
print(sum(squares))
# 列表解析,for循环这里不需要‘：’
squares = [value**3 for value in range(1, 9)]
print(squares)
```
结束
附言：有时候感觉一个东西学会之后就感觉简单了，是常识一般，往往不想记录下来，想去赶紧折腾新的东西。嗯，这种想法不太好，人总是会忘的，将来忘得时候可以回头看看自己写的东西，可以快速捡起来，毕竟自己和自己的脑回路相似。:>。