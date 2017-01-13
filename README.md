# DCE Setup for Alexandria Project

## For OSX local development
### Assumptions: homebrew for package management, rvm gemsets, OSX 10.12. YMMV with other setups.

1. git clone alexandria
1. Create a fresh gemset and `bundle install`
1. `brew install phantomjs`
1. install and start postgres
  * `brew install postgresql`
  * `initdb /usr/local/pgsql/data`
  * `pg_ctl -D /usr/local/pgsql/data -l logfile start`
1. configure hydra database in postgres - see defaults in config/database.yml
  * `createuser my_hydra_pg_user`
  * `createdb -O my_hydra_pg_user my_hydra_db`
  * TODO: How to record the password for hydra_pg_user?
  * Ensure you can connect: `psql -U my_hydra_pg_user -d my_hydra_db`
1. add a devise secret key (uncomment in config/initializers/devise.rb)
1. Install and start redis
  * `brew install redis` to install it the first time
  * in a new terminal window, run `redis-server /usr/local/etc/redis.conf`
1. Install and start marmotta
  * open new terminal window
  * `git clone https://github.com/curationexperts/marmotta-standalone`
  * start marmotta on 8080: `java -jar start.jar -Xmx1024mb -Djetty.port=8080`
1. setup marmotta to use postgres (see http://marmotta.apache.org/configuration.html)
  * Go to `http://localhost:8080/marmotta/storage-kiwi/admin/database.html`
  * Change values to read:
    * Database = postgres
    * Host = jdbc:postgresql://localhost:5432/my_hydra_db?prepareThreshold=3
    * User = my_hydra_pg_user
    * Password =
1. Run database migrations:
  * `rake db:migrate`
  * `rake db:migrate RAILS_ENV=test`
1. `cp config/secrets.yml.template config/secrets.yml`
1. `bundle exec rake ci` (will probably download solr & fedora) - this will error out every other time…
1. Make sure solr and fcrepo are running
  * `./bin/wrap`
  * visit `http://127.0.0.1:8985/solr/`
1. `bundle exec rails s`
1. connect to localhost:3000 in your browser
