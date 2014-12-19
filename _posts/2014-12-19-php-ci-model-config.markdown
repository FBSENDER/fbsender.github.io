---
layout: post
title:  "php CodeIgniter model config"
date:   2014-12-19
categories: php
---

网络间歇性抽搐，写了半天的东西不见了...做好备份的重要性

php CodeIgniter 连接数据库，构件model，串联起MVC三层

##php CodeIgniter demo

文件application/models/keyword_model.php:

```php
<?php
class Keyword_model extends CI_Model{
  public function __construct()
  {
    parent::__construct();
  }
  public function first_keyword(){
    $query = $this->db->get('seo_keywords',1);
    return $query->result();
  }
}
```

文件application/controllers/keyword.php:

```php
<?php
class Keyword extends CI_Controller{
  public function index(){
    $this->load->model('keyword_model','',TRUE);
    $data['results'] = $this->keyword_model->first_keyword();
    $this->load->view('k_message',$data);
  }
}
```

文件application/views/k_message.php:

```php
<?php echo $results[0]->id; ?>
```

这样访问 http://localhost:8080/keyword 就可以看到 表 seo_keywords 第一行的 id 了。

##数据库配置 database config

首先需要安装php mysql mod

ubuntu下使用下面指令安装：

```
sudo apt-get install php5-mysql
```

修改配置文件，application/config/database.php

##官方文档关于model的bug

Where Model_name is the name of your class. Class names must have the first letter capitalized with the rest of the name lowercase. Make sure your class extends the base Model class.

The file name must match the class name. For example, if this is your class:

```php
class User_model extends CI_Model {

        public function __construct()
        {
                parent::__construct();
        }

}
```
Your file will be this:

```
application/models/User_model.php
```

文件命名如果首字母大写，则程序会报错。
