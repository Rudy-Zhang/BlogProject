title: "CodeIgniter笔记之数据库访问"
date: 2014-10-28 
category: [web开发]
tags: [CodeIgniter,PHP]

---
####修改配置文件
`application/config/database.php`
####装载
将数据库访问对象，装载到超级对象属性中 $this->load->database();
####查询

```
$res->$this->db->query($sql) //返回查询结果对象对象
$res->result() ;// 返回数组，数组中是一个个对象
$res->result_array(); // 返回二维数组，里面是关联数组
$res->row(); // 返回第一条数据
```
####参数绑定

```
$sql="select * from blog_user where name=?";
$this->db->query($sql,$name); // 如果有多个参数（问号），需要使用索引数组
```
####db的自动加载
配置自动加载db `application/config/autoload.php`
```
$autoload['libaraied'] = array('database'); // 就不需要$this->load->database();
```
####CI的AR模型 Active Record
#####配置
`applicaion/config/database.php` 设置`$active_record = TRUE`
#####操作 

```
$res=$this->db->get('表名'); // 返回结果对象
$res->result();
$bool = $this->db->insert('表名',关联数组);
$bool=$this->db->update('表名',关联数组,条件);
$bool=$this->db->delete('表名',条件)
```
#####连贯操作
模拟sql语句

```
$res=$this->db->select('id,name')->from('user')->where('id>=3');
```
使用where

```
$res=$this->db->where('name','mary')->get('user');
```

#####复杂的查询使用参数绑定
`$this->db->query($sql,$data)`
####数据库查询结果
#####result()
方法执行成功返回一个对象数组，失败则返回一个空数组。 
例：

```
$query = $this->db->query("要执行的 SQL");

foreach ($query->result() as $row)
{
   echo $row->title;
   echo $row->name;
   echo $row->body;
}
```
#####判断结果集的条目数

```
if ($query->num_rows() > 0)
```
#####result_array()
该方法执行成功时将记录集作为关联数组返回。失败时返回空数组。

```
$query = $this->db->query("要执行的 SQL");

foreach ($query->result_array() as $row)
{
   echo $row['title'];
   echo $row['name'];
   echo $row['body'];
}

```
#####row()
该函数将当前请求的第一行数据作为 object 返回。
####总结，CI中的增删改查
#####查
- 得到一个表

```
$this->db->get('表名')
```

- 单个表条件查询

```
$this->db->get_where('表名','关联数组条件')
```

- 单表查询多个限定条件，链式查询
```
$this->db->select('title')->from('mytable')->where('id', $id)->limit(10, 20);
$query  = $this->db->get();
```

- 多表查询

```
$this ->db->query($sql,$data)
$sql   = "SELECT * FROM some_table WHERE id = ? AND status = ? AND author = ?";
$query = $this->admin->query($sql, array($id, $status, $author));
```

#####增
可以向函数传递 数组 或一个 对象。

```
$bool = $this->db->insert('表名',关联数组);
$data = array(
               'title' => 'My title' ,
               'name' => 'My Name' ,
               'date' => 'My date'
            );
$this->db->insert('mytable', $data); 
// Produces: INSERT INTO mytable (title, name, date) VALUES ('My title', 'My name', 'My date')
```
#####改

```
$bool=$this->db->update('表名',关联数组,条件);

$data = array(
               'title' => $title,
               'name' => $name,
               'date' => $date
            );

$this->db->where('id', $id);
$this->db->update('mytable', $data); 

```
或

```
$this->db->update('mytable', $data, array('id' => $id));
```
#####删

```
$bool =$this->db->delete('表名',条件)
```




