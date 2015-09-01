# influenced by the official docker ruby env
# https://github.com/docker-library/ruby/blob/master/2.0/Dockerfile
FROM orgsync/base-dev:1.0.0
MAINTAINER Joshua Griffith <joshua@orgsync.com>

# install ruby
ENV RUBY_MAJOR 2.0
ENV RUBY_VERSION 2.0.0-p647

RUN apt-get update \
    && apt-get install -y \
        bison \
        ruby \
    && mkdir -p /usr/src/ruby \
    && wget -q -O - http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.bz2 \
        | tar -xjC /usr/src/ruby --strip-components=1 \
    && cd /usr/src/ruby \
    && autoconf \
    && ./configure --disable-install-doc \
    && make -j"$(nproc)" \
    && apt-get purge -y --auto-remove bison ruby \
    && make install \
    && rm -r /usr/src/ruby \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# install rubygems
ENV RUBYGEMS_VERSION 2.4.8
ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH

# skip installing gem documentation
RUN echo 'gem: --no-rdoc --no-ri' >> "$HOME/.gemrc"

RUN gem install -v $RUBYGEMS_VERSION rubygems-update \
    && update_rubygems \
    && gem uninstall rubygems-update

# install bundler
ENV BUNDLER_VERSION 1.10.6
RUN gem install bundler -v $BUNDLER_VERSION \
	&& bundle config --global path "$GEM_HOME" \
	&& bundle config --global bin "$GEM_HOME/bin"

# don't create ".bundle" in all our apps
ENV BUNDLE_APP_CONFIG $GEM_HOME

CMD [ "irb" ]
