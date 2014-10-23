# influenced by the official docker ruby env
# https://github.com/docker-library/ruby/blob/master/2.0/Dockerfile
FROM orgsync/base-dev
MAINTAINER Clifton King <cliftonk@gmail.com>

# install ruby
ENV RUBY_MAJOR 2.1
ENV RUBY_VERSION 2.1.3
ENV RUBY_URL http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.bz2
ENV RUBY_SRC_DIR /usr/src/ruby

RUN apt-get update \
    && apt-get install -y \
        bison=1:2.5.dfsg-2.1 \
        ruby=1:1.9.3 \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p $RUBY_SRC_DIR \
    && wget -q -O - $RUBY_URL | tar -xjC $RUBY_SRC_DIR --strip-components=1 \
    && cd $RUBY_SRC_DIR \
    && autoconf \
    && ./configure --disable-install-doc \
    && make -j"$(nproc)" \
    && apt-get purge -y --auto-remove bison ruby \
    && make install

# install rubygems
ENV RUBYGEMS_VERSION 2.4.2
ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH

# skip installing gem documentation
RUN echo 'gem: --no-rdoc --no-ri' >> "$HOME/.gemrc"

RUN gem install -v $RUBYGEMS_VERSION rubygems-update \
    && update_rubygems \
    && gem uninstall rubygems-update

# install bundler
ENV BUNDLER_VERSION 1.7.4
RUN gem install bundler -v $BUNDLER_VERSION \
	&& bundle config --global path "$GEM_HOME" \
	&& bundle config --global bin "$GEM_HOME/bin"

# don't create ".bundle" in all our apps
ENV BUNDLE_APP_CONFIG $GEM_HOME

CMD [ "irb" ]
