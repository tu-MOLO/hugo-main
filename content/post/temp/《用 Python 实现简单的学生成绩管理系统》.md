# 《用 Python 实现简单的学生成绩管理系统》

以下是一个包含较长代码块的文本示例，以实现一个简单的学生成绩管理系统为例，使用 Python 语言，涵盖了类的定义、数据的增删改查等操作：

# 学生成绩管理系统实现

在教育领域，学生成绩管理系统是一个非常重要的工具，它可以帮助教师和管理人员高效地管理学生的成绩信息。下面我们通过 Python 代码来实现一个简单的学生成绩管理系统，该系统能够进行学生信息的录入、成绩的添加与修改、学生信息和成绩的查询等操作。

## 定义学生类

首先，我们定义一个 `Student` 类来表示学生对象，每个学生对象包含学生的姓名、学号和成绩信息。



```
class Student:

&#x20;   def \_\_init\_\_(self, student\_id, name):

&#x20;       self.student\_id = student\_id

&#x20;       self.name = name

&#x20;       self.scores = {}  # 用于存储课程名称和对应成绩的字典

&#x20;   def add\_score(self, course, score):

&#x20;       """

&#x20;       添加课程成绩

&#x20;       :param course: 课程名称

&#x20;       :param score: 课程成绩

&#x20;       """

&#x20;       if 0 <= score <= 100:

&#x20;           self.scores\[course] = score

&#x20;       else:

&#x20;           print(f"成绩输入错误，成绩应在 0 到 100 之间，{self.name} 的 {course} 成绩输入无效。")

&#x20;   def update\_score(self, course, new\_score):

&#x20;       """

&#x20;       更新课程成绩

&#x20;       :param course: 课程名称

&#x20;       :param new\_score: 新的课程成绩

&#x20;       """

&#x20;       if course in self.scores:

&#x20;           if 0 <= new\_score <= 100:

&#x20;               self.scores\[course] = new\_score

&#x20;           else:

&#x20;               print(f"成绩输入错误，成绩应在 0 到 100 之间，{self.name} 的 {course} 新成绩输入无效。")

&#x20;       else:

&#x20;           print(f"{self.name} 尚未录入 {course} 成绩，无法更新。")

&#x20;   def get\_score(self, course):

&#x20;       """

&#x20;       获取课程成绩

&#x20;       :param course: 课程名称

&#x20;       :return: 课程成绩，如果未录入则返回 None

&#x20;       """

&#x20;       return self.scores.get(course)

&#x20;   def get\_all\_scores(self):

&#x20;       """

&#x20;       获取所有课程成绩

&#x20;       :return: 包含所有课程和成绩的字典

&#x20;       """

&#x20;       return self.scores

&#x20;   def \_\_str\_\_(self):

&#x20;       """

&#x20;       返回学生信息的字符串表示

&#x20;       """

&#x20;       score\_str = ", ".join(\[f"{course}: {score}" for course, score in self.scores.items()])

&#x20;       return f"学号: {self.student\_id}, 姓名: {self.name}, 成绩: {score\_str}"
```

## 定义成绩管理系统类

接着，我们定义一个 `GradeManagementSystem` 类来管理多个学生对象，实现学生的添加、删除、查询以及成绩的管理等功能。



```
class GradeManagementSystem:

&#x20;   def \_\_init\_\_(self):

&#x20;       self.students = {}  # 用于存储学生对象的字典，以学号为键

&#x20;   def add\_student(self, student\_id, name):

&#x20;       """

&#x20;       添加学生

&#x20;       :param student\_id: 学生学号

&#x20;       :param name: 学生姓名

&#x20;       """

&#x20;       if student\_id not in self.students:

&#x20;           self.students\[student\_id] = Student(student\_id, name)

&#x20;           print(f"成功添加学生 {name}，学号为 {student\_id}。")

&#x20;       else:

&#x20;           print(f"学号为 {student\_id} 的学生已存在，无法重复添加。")

&#x20;   def remove\_student(self, student\_id):

&#x20;       """

&#x20;       删除学生

&#x20;       :param student\_id: 学生学号

&#x20;       """

&#x20;       if student\_id in self.students:

&#x20;           del self.students\[student\_id]

&#x20;           print(f"成功删除学号为 {student\_id} 的学生。")

&#x20;       else:

&#x20;           print(f"学号为 {student\_id} 的学生不存在，无法删除。")

&#x20;   def get\_student(self, student\_id):

&#x20;       """

&#x20;       获取学生对象

&#x20;       :param student\_id: 学生学号

&#x20;       :return: 学生对象，如果不存在则返回 None

&#x20;       """

&#x20;       return self.students.get(student\_id)

&#x20;   def list\_students(self):

&#x20;       """

&#x20;       列出所有学生信息

&#x20;       """

&#x20;       if self.students:

&#x20;           for student in self.students.values():

&#x20;               print(student)

&#x20;       else:

&#x20;           print("当前没有学生信息。")

&#x20;   def add\_student\_score(self, student\_id, course, score):

&#x20;       """

&#x20;       为学生添加课程成绩

&#x20;       :param student\_id: 学生学号

&#x20;       :param course: 课程名称

&#x20;       :param score: 课程成绩

&#x20;       """

&#x20;       student = self.get\_student(student\_id)

&#x20;       if student:

&#x20;           student.add\_score(course, score)

&#x20;       else:

&#x20;           print(f"学号为 {student\_id} 的学生不存在，无法添加成绩。")

&#x20;   def update\_student\_score(self, student\_id, course, new\_score):

&#x20;       """

&#x20;       更新学生课程成绩

&#x20;       :param student\_id: 学生学号

&#x20;       :param course: 课程名称

&#x20;       :param new\_score: 新的课程成绩

&#x20;       """

&#x20;       student = self.get\_student(student\_id)

&#x20;       if student:

&#x20;           student.update\_score(course, new\_score)

&#x20;       else:

&#x20;           print(f"学号为 {student\_id} 的学生不存在，无法更新成绩。")

&#x20;   def get\_student\_score(self, student\_id, course):

&#x20;       """

&#x20;       获取学生课程成绩

&#x20;       :param student\_id: 学生学号

&#x20;       :param course: 课程名称

&#x20;       :return: 课程成绩，如果学生不存在或未录入该课程成绩则返回 None

&#x20;       """

&#x20;       student = self.get\_student(student\_id)

&#x20;       if student:

&#x20;           return student.get\_score(course)

&#x20;       else:

&#x20;           print(f"学号为 {student\_id} 的学生不存在，无法获取成绩。")

&#x20;           return None

&#x20;   def get\_student\_all\_scores(self, student\_id):

&#x20;       """

&#x20;       获取学生所有课程成绩

&#x20;       :param student\_id: 学生学号

&#x20;       :return: 包含所有课程和成绩的字典，如果学生不存在则返回 None

&#x20;       """

&#x20;       student = self.get\_student(student\_id)

&#x20;       if student:

&#x20;           return student.get\_all\_scores()

&#x20;       else:

&#x20;           print(f"学号为 {student\_id} 的学生不存在，无法获取所有成绩。")

&#x20;           return None
```

## 测试成绩管理系统

最后，我们编写一些测试代码来验证这个学生成绩管理系统的功能。



```
\# 创建成绩管理系统实例

system = GradeManagementSystem()

\# 添加学生

system.add\_student(1, "Alice")

system.add\_student(2, "Bob")

\# 添加学生成绩

system.add\_student\_score(1, "Math", 90)

system.add\_student\_score(1, "English", 85)

system.add\_student\_score(2, "Math", 80)

\# 列出所有学生信息

system.list\_students()

\# 更新学生成绩

system.update\_student\_score(1, "Math", 95)

\# 获取学生成绩

print(f"Alice 的 Math 成绩: {system.get\_student\_score(1, 'Math')}")

\# 获取学生所有成绩

print(f"Bob 的所有成绩: {system.get\_student\_all\_scores(2)}")

\# 删除学生

system.remove\_student(2)

system.list\_students()
```

通过以上代码，我们实现了一个基本的学生成绩管理系统，涵盖了学生信息和成绩的常见操作。在实际应用中，可以根据需求进一步扩展和优化这个系统，例如添加数据持久化功能、用户界面等。