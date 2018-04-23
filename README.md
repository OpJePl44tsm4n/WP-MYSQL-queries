# WP-MYSQL-queries

## Update all references from the old to the new url
```sql
SET @db_prefix := 'wp_';
SET @old_site := '**{old_site_url}**';
SET @new_site := '**{new_site_url}**;
SET @options := concat(@db_prefix,'options');
SET @posts := concat(@db_prefix,'posts');

SET @qry1 := CONCAT('UPDATE ', @options, ' SET option_value = replace(option_value, ''', @old_site, ''', ''', @new_site, ''') WHERE option_name = ''home'' OR option_name = ''siteurl'';');
prepare stmt from @qry1 ;
execute stmt ;

SET @qry2 := CONCAT('UPDATE ', @posts, ' SET guid = replace(guid, ''', @old_site, ''', ''', @new_site, ''');');
prepare stmt from @qry2 ;
execute stmt ;

SET @qry3 := CONCAT('UPDATE ', @posts, ' SET post_content = replace(post_content, ''', @old_site, ''', ''', @new_site, ''');');
prepare stmt from @qry3;
execute stmt;
```

### for multi site you also need to update the 'site' and 'blogs' tabels 
```sql
SET @db_prefix := 'wp_';
SET @old_site := '**{old_site_url}**';
SET @new_site := '**{new_site_url}**;
SET @blogs := concat(@db_prefix,'blogs');
SET @site := concat(@db_prefix,'site');

SET @qry4 := CONCAT('UPDATE ', @blogs, ' SET domain = replace(domain, ''', @old_site, ''', ''', @new_site, ''');');
prepare stmt from @qry4 ;
execute stmt ;

SET @qry5 := CONCAT('UPDATE ', @site, ' SET domain = replace(domain, ''', @old_site, ''', ''', @new_site, ''');');
prepare stmt from @qry5;
execute stmt;
```
