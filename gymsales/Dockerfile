FROM ruby:2.3.7

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -

RUN apt-get update && apt-get install -y \
  tar \
  build-essential \
  nodejs \
  npm \
  openssl \
  libpq-dev

ENV RAILS_ROOT /webapp
ENV POSTGRES_HOST_AUTH_METHOD=md5

RUN mkdir -p  $RAILS_ROOT
WORKDIR $RAILS_ROOT

# # Setting env up
# ENV RAILS_ENV='development'

# # Adding gems
COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock
# RUN bundle config --global frozen 1 \
#     && bundle install --deployment --without development:test:assets -j4 --path=vendor/bundle \
#     && rm -rf vendor/bundle/ruby/2.3.0/cache/*.gem \
#     && find vendor/bundle/ruby/2.3.0/gems/ -name "*.c" -delete \
#     && find vendor/bundle/ruby/2.3.0/gems/ -name "*.o" -delete
RUN bundle install --jobs 20 --retry 5 --without development test

RUN npm install -g cspell@3.2.17

# monkey patch activerecord gem to support PostgreSQL 12+
RUN sed -i "s/client_min_messages, 'panic'/client_min_messages, 'warning'/g" /usr/local/bundle/gems/activerecord-4.1.16/lib/active_record/connection_adapters/postgresql_adapter.rb

RUN curl "https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.0/wkhtmltox-linux-amd64_0.12.0-03c001d.tar.xz" -L -o "wkhtmltopdf.tar.xz"
RUN tar Jxvf wkhtmltopdf.tar.xz
RUN mv wkhtmltox/bin/wkhtmltopdf /usr/local/bin/wkhtmltopdf

# vi bindings for readline
RUN echo "set editing-mode vi\n\
set keymap vi\n\
set show-mode-in-prompt on\n\
"_": beginning-of-line" > "/etc/inputrc"

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3333

CMD ["rails", "server", "-b", "0.0.0.0"]
