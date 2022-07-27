docker volume create --name=gymsales_rails_cache && docker volume create --name=gymsales_bundle
docker volume create --name=fitness-sync
docker network create gymsales_link

docker compose build --no-cache
docker compose up

docker exec -it postgres bash

docker compose run --rm app bundle install
docker compose run --rm app bundle exec rake db:create db:create_functions db:schema:load db:migrate


docker compose run --rm app bundle exec pg_dump -h gymsales-prod-small.chbtqmkfospg.ap-southeast-2.rds.amazonaws.com -Fc -o -U master gymsales > gymsales.dump

docker compose run --rm app bundle exec rails c
