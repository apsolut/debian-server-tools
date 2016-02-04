uwp download core

# Directory structure

```
docroot─┬─index.php
        ├─wp-config.php
        ├─$CORE─┬─wp-load.php
        │       ├─wp-login.php
        │       ├─wp-admin/
        │       └─wp-includes/
        └─static/
```

wp-cli.yml options: path, user ...

./wp-createdb.sh

```bash
wp core config --dbname="$DBNAME" --dbuser="$DBUSER" --dbpass="$DBPASS" \
    --dbhost="$DBHOST" --dbprefix="prod" --dbcharset="$DBCHARSET" --extra-php <<EOF
EOF

wp core install --url="${WPHOMEURL}/${COMPANY}" --title="Site Title" \
    --admin_user="$ME" --admin_password="$MYPASS" --admin_email=viktor@szepe.net

wp option set home "$WPHOMEURL"
wp option set blog_public "0"
wp option set admin_email "support@company.net"
```

# Redis object cache

```bash
apt-get install redis-server
pecl install redis
echo -e "; priority=20\nextension=redis.so" > /etc/php5/mods-available/redis.ini
php5enmod redis && php -m|grep redis
# PHP 7
#echo -e "; priority=20\nextension=redis.so" > /etc/php/mods-available/redis.ini
#phpenmod -v 7.0 -s ALL redis && php -m|grep redis
cd /home/wp/
wp plugin install wp-redis
ln -sv plugins/wp-redis/object-cache.php static/
composer create-project -s dev erik-dubbelboer/php-redis-admin radmin
```

In wp-config.php:

```php
define( 'WP_CACHE_KEY_SALT', 'prod_' );
/*
$redis_server = array( 'host' => '127.0.0.1',
                       'port' => 6379
);
*/
```

### Security

- fail2ban
- wpf2b



### On deploy / Staging->Production

- `wp transient delete-all`
- `wp db query "DELETE FROM $(wp eval 'global $table_prefix;echo $table_prefix;')options WHERE option_name LIKE '%_transient_%'"`
- Remove development wp_options -> Option Inspector
- Delete unused Media files @TODO `for $m in files; search $m in DB;`
- `wp db optimize`
- WP Cleanup plugin

### Settings

- General Settings
- Writing Settings
- Reading Settings
- Media Settings
- Permalink Settings
- WP Mail From

#### Search & replace items

`wp search-replace --precise --recurse-objects --all-tables-with-prefix`

1. http://DOMAIN.TLD or https (no trailing slash)
1. /home/PATH/TO/SITE (no trailing slash)
1. EMAIL@ADDRESS.ES (all addresses)
1. DOMAIN.TLD (now without http)

Manual replace constants in `wp-config.php`.

### Move a site to subdirectories

1. siteurl += /site
1. search-and-replace: /wp-includes/ -> /site/wp-includes/
1. search-and-replace: /wp-content/ -> /static/







@TODO wp-lib