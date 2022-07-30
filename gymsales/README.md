docker volume create --name=gymsales_rails_cache && docker volume create --name=gymsales_bundle
docker volume create --name=fitness-sync
docker network create gymsales_link

docker compose build --no-cache
docker compose run --rm app bundle exec rake db:create db:create_functions db:schema:load db:migrate

docker exec -it gymsales_app_db bash
psql -U postgres < /home/data_container/gymsales.sql 

docker-compose up -d && docker attach $(docker-compose ps -q app)

docker compose run --rm app bundle exec rails c
