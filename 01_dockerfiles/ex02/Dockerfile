# Load base dev image
FROM ruby:2.7-rc-alpine

# Install dev libs/dep & create new rails app & clean
RUN apk add --no-cache \
	tar \
	build-base \
	tzdata \
	sqlite-dev \
	nodejs \
	yarn 
RUN gem install tzinfo-data && gem install rails && rails new app && cd app && bundle exec rake assets:precompile
# RUN pwd && ls
RUN rm -rf /usr/local/bundle/cache/*.gem \
 && find /usr/local/bundle/gems/ -name "*.c" -delete \
 && find /usr/local/bundle/gems/ -name "*.o" -delete \
 && rm -rf node_modules tmp log public/packs public/packs-test public/assets app/assets vendor/assets lib/assets spec yarn.lock

# Load base app image
FROM ruby:2.7-rc-alpine

# Set working dir
WORKDIR /opt/app

# Copy app and dep from dev container
COPY --from=0 --chown=1000:1000 /usr/local/bundle/ /usr/local/bundle/
COPY --from=0 --chown=1000:1000 /app /opt/app

# Install missing production deps and db migrations & populations
RUN apk add --no-cache \
	tzdata \
	sqlite-libs \
 && rake db:migrate && rake db:seed

# Expose port
EXPOSE 3000

# Launch server on run
CMD ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
