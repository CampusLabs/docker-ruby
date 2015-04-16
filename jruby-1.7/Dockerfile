FROM orgsync/java8:1.8.0_45
MAINTAINER Joshua Griffith

# install jruby
ENV JRUBY_VERSION 1.7.19

RUN mkdir /opt/jruby \
  && wget -q -O - http://jruby.org.s3.amazonaws.com/downloads/${JRUBY_VERSION}/jruby-bin-${JRUBY_VERSION}.tar.gz \
      | tar -zxC /opt/jruby --strip-components=1 \
  && update-alternatives --install /usr/local/bin/ruby ruby /opt/jruby/bin/jruby 1
ENV PATH /opt/jruby/bin:$PATH

# skip installing gem documentation
RUN echo 'gem: --no-rdoc --no-ri' >> $HOME/.gemrc

# install rubygems
ENV RUBYGEMS_VERSION 2.4.6
ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH

RUN gem install -v $RUBYGEMS_VERSION rubygems-update \
    && update_rubygems \
    && gem uninstall -x rubygems-update

# install bundler
ENV BUNDLER_VERSION 1.9.4
RUN gem install bundler -v $BUNDLER_VERSION \
	&& bundle config --global path "$GEM_HOME" \
	&& bundle config --global bin "$GEM_HOME/bin"

# don't create ".bundle" in all our apps
ENV BUNDLE_APP_CONFIG $GEM_HOME

CMD [ "irb" ]
