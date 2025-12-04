This is a reproducer for https://github.com/sulu/sulu/issues/7713

What did I change:
* Basically, I changed the docker compose config a bit to run it without a local PHP installation.
* I updated several composer packages and added handcraftedinthealps/elasticsearch-bundle

Steps to run:
* Run `docker compose up -d`
* Run `docker-compose exec php bash`
* Run `php -dmemory_limit=4G /usr/local/bin/composer install`
* Look at the output of the last command
* An error is shown
    * Incompatible use of dynamic environment variables "SENTRY_DSN" found in parameters.

I believe the reason is that in vendor/sentry/sentry/src/Dsn.php::createFromString no valid url is passed.
I only got placeholders like "env_bb23b29f1bdc2b7c_SENTRY_DSN_8f8a5956fb7cdb83e4d95d56d1c2bda4" there.
This seems to cause exceptions, which in turn seem to trigger the "Incompatible" message.
