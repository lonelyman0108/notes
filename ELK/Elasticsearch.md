## Elasticsearch

### 修改密码

#### 命令

重置为随机密码

```bash
cd /usr/share/elasticsearch/bin
elasticsearch-reset-password -u <username>
```

重置为自定义密码

```http
POST /_security/user/<username>/_password
{
  "password": "<password>"
}
------
{}
```

