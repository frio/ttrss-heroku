# Tiny Tiny RSS for Heroku
This is a default distribution of [Tiny Tiny RSS][1] which can be run directly on Heroku (or dokku!).  It currently tracks version 1.12, and is inspired by the [Project Delphai][2] script.

[1]: http://tt-rss.org
[2]: https://github.com/projectdelphai/ttrss-on-heroku/


## Installation
To get up and running, you'll need to create a database, populate it, and install the TT-RSS schema.  Assuming you have the heroku toolbelt and git:

    git clone http://github.com/frio/ttrss-heroku
    cd ttrss-heroku
    heroku create
    heroku addons:add heroku-postgresql:dev
    cat schema/ttrss_schema_pgsql.sql | heroku pg:psql `heroku pg:info | sed 's/=== HEROKU_POSTGRESQL_//' | sed 's/_URL (DATABASE_URL)//' | sed 's/_URL//' | head -n 1`

The application should be almost ready to go at this point.  We'll configure it first.

    heroku config:set SECRET_KEY=`openssl rand -base64 24` # or another random key
    heroku config:set DATABASE_URL=$your_postgres_url # you can get this using heroku config:get
    heroku config:set APP_URL="http://my-amazing-app.herokuapp.com"

And, finally, push it live, scale it up and visit!

    git push heroku master
    heroku ps:scale web=1
    heroku ps:scale updater=1
    heroku open


## TODO
* Verify the updater runs properly
* Rebase on TT-RSS' git repository


## License of TT-RSS
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

Copyright (c) 2005 Andrew Dolgov (unless explicitly stated otherwise).

Uses Silk icons by Mark James: http://www.famfamfam.com/lab/icons/silk/

