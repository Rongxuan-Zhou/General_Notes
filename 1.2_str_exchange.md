从最基础开始，逐步深入介绍字符串替换：

1. 最简单的替换 - str.replace()：
```python
# 基础替换
text = "Hello World"
new_text = text.replace("World", "Python")
print(new_text)  # Hello Python

# 限制替换次数
text = "hello hello hello"
new_text = text.replace("hello", "hi", 2)
print(new_text)  # hi hi hello
```

2. 使用正则表达式简单替换 - re.sub()：
```python
import re

# 替换数字
text = "电话123手机456"
new_text = re.sub(r'\d', 'X', text) # text为目标target
print(new_text)  # 电话XXX手机XXX

# 替换指定次数
text = "电话123手机456"
new_text = re.sub(r'\d', 'X', text, count=2) # 第一个参数为正则规则，第二个为替换内容的字符串，第三个为目标字符串，第四个为参数最多替换次数
print(new_text)  # 电话XX3手机456
```

3. 使用模式匹配替换：
```python
import re

# 匹配电话号码格式
text = "联系电话：123-456-7890 和 987-654-3210"
pattern = r'\d{3}-\d{3}-\d{4}'
new_text = re.sub(pattern, '***-***-****', text)
print(new_text)  # 联系电话：***-***-**** 和 ***-***-****

# 匹配邮箱
text = "邮箱是: user@example.com 和 admin@test.com"
pattern = r'[\w\.-]+@[\w\.-]+'
new_text = re.sub(pattern, '[邮箱已隐藏]', text)
print(new_text)  # 邮箱是: [邮箱已隐藏] 和 [邮箱已隐藏]
```

4. 使用函数进行替换：
```python
import re

# 使用函数处理匹配的内容
def convert_number(match):
    num = int(match.group())
    return str(num * 2)

text = "数字：1 2 3 4 5"
new_text = re.sub(r'\d+', convert_number, text)
print(new_text)  # 数字：2 4 6 8 10

# 复杂的替换逻辑
def mask_phone(match):
    phone = match.group()
    return f"{phone[:3]}****{phone[-4:]}"

text = "电话：13812345678"
new_text = re.sub(r'1\d{10}', mask_phone, text)
print(new_text)  # 电话：138****5678
```

5. 高级替换技巧：
```python
import re

# 分组引用
text = "姓名: Zhang, Age: 25"
pattern = r'(\w+): (\w+)'
new_text = re.sub(pattern, r'\2: \1', text)
print(new_text)  # Zhang: 姓名, 25: Age

# 条件替换
def conditional_replace(match):
    num = int(match.group())
    return str(num * 2) if num > 5 else match.group()

text = "1 2 3 6 7 8"
new_text = re.sub(r'\d+', conditional_replace, text)
print(new_text)  # 1 2 3 12 14 16
```

6. 实际应用示例：
```python
import re

# 数据清洗
def clean_text(text):
    # 移除多余空格
    text = re.sub(r'\s+', ' ', text)
    # 统一日期格式
    text = re.sub(r'(\d{4})-(\d{2})-(\d{2})', r'\1/\2/\3', text)
    # 替换特殊字符
    text = re.sub(r'[#@$%]', '', text)
    return text.strip()

text = "2023-12-25   这是一个#测试@文本"
print(clean_text(text))  # 2023/12/25 这是一个测试文本

# 敏感信息处理
def mask_sensitive_info(text):
    # 手机号码
    text = re.sub(r'1\d{10}', lambda m: m.group()[:3] + '****' + m.group()[-4:], text)
    # 身份证号
    text = re.sub(r'\d{17}[\dX]', lambda m: m.group()[:6] + '********' + m.group()[-4:], text)
    # 邮箱
    text = re.sub(r'[\w\.-]+@[\w\.-]+', lambda m: m.group().split('@')[0][:3] + '***@' + m.group().split('@')[1], text)
    return text

sensitive_text = "电话：13812345678，邮箱：user@example.com"
print(mask_sensitive_info(sensitive_text))
# 输出：电话：138****5678，邮箱：use***@example.com
```
key points：
1. 简单替换用str.replace()
2. 需要模式匹配时用re.sub()
3. 复杂逻辑用函数替换
4. 注意转义字符和特殊字符
5. 合理使用分组和引用
