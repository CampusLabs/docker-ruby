FROM quay.io/orgsync/java:1.8
MAINTAINER Joshua Griffith

# install jruby
ENV JRUBY_VERSION 9.0.5.0
ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH

RUN mkdir /opt/jruby \
  && wget -q -O - http://jruby.org.s3.amazonaws.com/downloads/${JRUBY_VERSION}/jruby-bin-${JRUBY_VERSION}.tar.gz \
      | tar -zxC /opt/jruby --strip-components=1 \
  && update-alternatives --install /usr/local/bin/ruby ruby /opt/jruby/bin/jruby 1
ENV PATH /opt/jruby/bin:$PATH

# skip installing gem documentation
RUN echo 'gem: --no-rdoc --no-ri' >> $HOME/.gemrc

# install bundler
ENV BUNDLER_VERSION 1.11.2
RUN gem install bundler -v $BUNDLER_VERSION \
	&& bundle config --global path "$GEM_HOME" \
	&& bundle config --global bin "$GEM_HOME/bin"

# don't create ".bundle" in all our apps
ENV BUNDLE_APP_CONFIG $GEM_HOME

CMD [ "irb" ]
