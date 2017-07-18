# This Source Code Form is subject to the terms of the Mozilla Public
# License, v. 2.0. If a copy of the MPL was not distributed with this
# file, You can obtain one at http://mozilla.org/MPL/2.0/.
#
# This Source Code Form is "Incompatible With Secondary Licenses", as
# defined by the Mozilla Public License, v. 2.0.

FROM mozillaconduit/docker-bmo
MAINTAINER David Lawrence <dkl@mozilla.com>

ENV BUGZILLA_ROOT /home/bugzilla/devel/htdocs/bmo

# Packages
COPY conf/rpm_list /rpm_list
RUN yum -y install `cat /rpm_list` && yum clean all

# Copy setup scripts
COPY scripts/* /usr/local/bin/
RUN chmod 755 /usr/local/bin/*

# Apache setup
COPY conf/bugzilla.conf /etc/httpd/conf.d/bugzilla.conf

# Development environment setup
RUN git clone $GITHUB_BASE_GIT -b $GITHUB_BASE_BRANCH --depth 1 $BUGZILLA_ROOT \
    && ln -sf $BUGZILLA_LIB $BUGZILLA_ROOT/local
COPY conf/checksetup_answers.txt $BUGZILLA_ROOT/checksetup_answers.txt
RUN bugzilla_config.sh
RUN su - $BUGZILLA_USER -c dev_config.sh

RUN chown -R $BUGZILLA_USER.$BUGZILLA_USER /home/$BUGZILLA_USER
