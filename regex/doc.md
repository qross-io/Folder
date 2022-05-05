# 模块文档

## 数据表结构 

```sql
CREATE TABLE `starc_regex_seek_help`  (
    `id` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `guid` VARCHAR(50),
    `email` VARCHAR(255),
    `origin_text` VARCHAR(4000),
    `iffy_regex` VARCHAR(1000),
    `question` VARCHAR(2000),
    `right_regex` VARCHAR(1000) DEFAULT '',
    `comment` VARCHAR(4000) DEFAULT '',
    `resolved` VARCHAR(100) DEFAULT 'no' COMMENT 'no/yes',
    create_time DATETIME DEFAULT NOW(),
    update_time DATETIME DEFAULT NOW() ON UPDATE NOW(),
    resolve_time DATETIME,
    KEY `starc_regex_seek_help_email` (`email`),
    KEY `starc_regex_seek_help_guid` (`guid`)
);

CREATE TABLE `starc_pageviews`  (
    `id` BIGINT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `guid` VARCHAR(50),
    `page` VARCHAR(1000),
    `mobile` VARCHAR(100) DEFAULT 'no',
    `visit_time` DATETIME DEFAULT NOW()
);
```