docker 安装(https://www.vultr.com/docs/install-the-postgis-extension-for-postgresql-on-ubuntu-linux)
```shell

安装 gis:
sudo apt install postgis postgresql-12-postgis-3
apt-get install postgresql-12-postgis-3-scripts

选择数据库>sql
CREATE EXTENSION postgis;
```





全文检索

https://github.com/hightman/scws

```shell
# install bzip2
# cd /opt
# wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2
# tar -jxvf ./scws-1.2.3.tar.bz2
# cd scws-1.2.3/
# ./configure 
# make 
# make install

# ldconfig
```

https://github.com/amutu/zhparser



apt install -y postgresql-server-dev-12



```
PG_CONFIG=/usr/lib/postgresql/13/bin/pg_config make 
PG_CONFIG=/usr/lib/postgresql/13/bin/pg_config make install
```

https://mlog.club/article/2514326

 **CREATE** **INDEX** mytable_description_tsvector_idx **ON** mytable(to_tsvector(description))  **WHERE** created_at > **now**() - '1 year'::interval;







```sql
-- 安装扩展
CREATE EXTENSION zhparser;
-- 中文分词配置
CREATE TEXT SEARCH CONFIGURATION chinese_parser (PARSER = zhparser);
ALTER TEXT SEARCH CONFIGURATION chinese_parser ADD MAPPING FOR n,v,a,i,e,l,j WITH simple;
-- create test table
CREATE TABLE text_search(
  text_id SERIAL PRIMARY KEY,
	text_name VARCHAR(32),
  text_content TEXT,
  tsv_column tsvector
);
-- 创建gin_index以加快匹配速度
CREATE INDEX idx_gin_tsv ON text_search USING GIN(tsv_column);
-- 创建触发器以从目标同步tsvector列
CREATE TRIGGER sync_trigger 
   BEFORE INSERT OR UPDATE ON text_search FOR EACH ROW 
   EXECUTE PROCEDURE 
   tsvector_update_trigger(tsv_column, 'public.chinese_parser', text_name , text_content);
-- 插入数据
INSERT INTO text_search(text_name,text_content) VALUES ('黄国宋','ThinkPad 小红点多功能蓝牙键盘多平台WIN安卓IOS支持 4X30K12182');
INSERT INTO text_search(text_name,text_content) VALUES ('陈家豪','多平台 富文本 写作软件');
-- 搜索关键词
SELECT * FROM text_search WHERE tsv_column @@ to_tsquery('chinese_parser', '多平台黄国宋');
SELECT * FROM text_search WHERE tsv_column @@ to_tsquery('chinese_parser', '小红点&多平台');


select CONCAT(text_name ,',' , text_content) FROM text_search

```

