FROM ruby:2.5

RUN apt-get update -qq && \
    apt-get install -y build-essential nodejs

WORKDIR /app

RUN useradd -u 1000 -Um rails && \
    chown -R rails:rails /app
USER rails

COPY Gemfile /app/
COPY Gemfile.lock /app/
RUN bundle install

COPY --chown=rails:rails . /app/
