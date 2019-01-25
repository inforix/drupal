# 1. 启用 *migrate_plus、migrate_tools* 模块

```bash
cd /var/www/html
composer require drupal/migrate_plus
composer require drupal/migrate_tools
vendor/drush/drush/drush -y en migrate_plus migrate_tools
```

# 2. 创建一个导入信息的模块

**模块名称**: migrate_json

File: migrate_json.info.yml

```yml
type: module
name: Migrate JSON file
description: 'Migrate JSON file'

dependencies:
  - drupal: migrate
  - migrate_plus: migrate_plus
  - migrate_tools: migrate_tools

version: '8.x-1.0'
core: '8.x'
```

```bash
mkdir -p /var/www/html/modules/migrate-json
mkdir -p /var/www/html/modules/migrate-json/config/install
```

File: /var/www/html/modules/migrate-json/config/install/migrate_plus.migration.articles.yml

```yml
id: jsonarticles
label: JSON feed of Articles
migration_group: Custom Articles
migration_tags:
  - json articles
source:
  plugin: url
  data_fetcher_plugin: file
  data_parser_plugin: json
  urls:
    - 'public://articles.json'
  item_selector: articles
  fields:
    -
      name: id
      label: ID
      selector: id
    -
      name: title
      label: 'Title'
      selector: title
    -
      name: created
      label: 'Created Date'
      selector: created
    -
      name: subtitle
      label: Subtitle
      selector: subtitle
    -
      name: body
      label: Body
      selector: body
  ids:
    id:
      type: integer
process:
  type:
    plugin: default_value
    default_value: article
  title: title
  status:
    plugin: default_value
    default_value: 1
  created:
    plugin: format_date
    from_format: 'Y-m-d H:i:s'
    to_format: 'U'
    source: created
  changed:
    plugin: format_date
    from_format: 'Y-m-d H:i:s'
    to_format: 'U'
    source: created
  'body/format':
    plugin: default_value
    default_value: "full_html"
  'body/value': body
  uid:
    plugin: default_value
    default_value: 1
  field_subtitle: subtitle
destination:
  plugin: entity:node
migration_dependencies: {}
```

articles.json 文件内容如下：

```json
{
  "articles": [
    {
      "id" : 1,
      "title": "Title 1",
      "created": "2018-10-01 23:02:59",
      "subtitle": "Sub Title",
      "body" : "<p>Body</p><p>Paragraph 2</p>"
    },
    {
      "id": 2,
      "title": "Title 2",
      "created": "2018-10-01 23:05:02",
      "subtitle": "Sub Title 2",
      "body" : "<p>Body 2</p><p>Paragraph 2</p>"
    },
  ]
}
```

# 3. 启用该模块

```bash
drush -y en migrate_json
```

# 4. 导入数据

```bash
# 查看状态
drush migrate-status
# 导入文章
drush migrate-import articles
# 回归刚才的导入
drush migrate-rollback articles
```