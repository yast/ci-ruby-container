
#!BuildTag: yast-ruby

# This actually is registry.opensuse.org/yast/head/images/opensuse/tumbleweed:latest
# when building in OBS YaST:Head!
FROM opensuse/tumbleweed

# do not install the files marked as documentation (use "rpm --excludedocs")
RUN sed -i -e "s/^.*rpm.install.excludedocs.*/rpm.install.excludedocs = yes/" /etc/zypp/zypp.conf

# import the YaST OBS GPG key
COPY YaST:Head.pub /usr/share/gpg-keys/
RUN rpm --import /usr/share/gpg-keys/YaST:Head.pub

# Prefer the packages from the YaST:Head repository
# TODO -p 50 does not work, but should be usless anyway, just like --refresh
RUN zypper addrepo --refresh http://download.opensuse.org/repositories/YaST:/Head/openSUSE_Tumbleweed yast

RUN zypper --non-interactive install --no-recommends \
  brp-check-suse \
  aspell-en \
  fdupes \
  git \
  grep \
  glibc-locale \
  rpm-build \
  update-desktop-files \
  which \
  libxml2-tools \
  libxslt-tools \
  "rubygem(ruby:3.4.0:abstract_method)" \
  "rubygem(ruby:3.4.0:cfa)" \
  "rubygem(ruby:3.4.0:cheetah)" \
  "rubygem(ruby:3.4.0:gettext)" \
  "rubygem(ruby:3.4.0:parallel)" \
  "rubygem(ruby:3.4.0:parallel_tests)" \
  "rubygem(ruby:3.4.0:raspell)" \
  "rubygem(ruby:3.4.0:rspec)" \
  "rubygem(ruby:3.4.0:rubocop:1.24.1)" \
  "rubygem(ruby:3.4.0:simplecov)" \
  "rubygem(ruby:3.4.0:simplecov-lcov)" \
  "rubygem(ruby:3.4.0:simpleidn)" \
  "rubygem(ruby:3.4.0:yard)" \
  "rubygem(ruby:3.4.0:yast-rake)" \
  build \
  obs-service-source_validator \
  openSUSE-release-ftp \
  patterns-rpm-macros \
  ShellCheck \
  suseconnect-ruby-bindings \
  trang \
  yast2 \
  yast2-add-on \
  yast2-bootloader \
  yast2-core \
  yast2-country \
  yast2-devtools \
  yast2-hardware-detection \
  yast2-installation \
  yast2-installation-control \
  yast2-ldap \
  yast2-network \
  yast2-nfs-server \
  yast2-packager \
  yast2-pam \
  yast2-perl-bindings \
  yast2-pkg-bindings \
  yast2-proxy \
  yast2-ruby-bindings \
  yast2-security \
  yast2-services-manager \
  yast2-slp \
  yast2-storage-ng \
  yast2-testsuite \
  yast2-transfer \
  yast2-update \
  yast2-users \
  yast2-xml \
  yast2-ycp-ui-bindings \
  && zypper clean --all \
  && rm -rf /usr/lib*/ruby/gems/*/cache/ \
  && rm -rf /usr/share/doc/ \
  && rpm -e --nodeps kbd \
  && find /usr/lib/locale/* -maxdepth 1 | grep -v -E "(en_US|cs_CZ|es_ES|de_DE|C.utf8)" | xargs rm -rf \
  && find /usr/share/locale -name "*.mo" -delete

# fail when there are multiple Ruby interpreters installed
# Because it means one part of YaST wanted a different version than the other
# and requires/imports will fail on the version mismatch.
RUN if [ `rpm -q --whatprovides "ruby(abi)" | wc -l` -gt 1 ]; then \
  rpm -q --whatprovides "ruby(abi)"; \
  echo "Multiple Rubies detected, most likely the system Ruby version has been upgraded."; \
  echo "Update the versions of the included Ruby gems."; exit 1; fi

COPY yast-ci-ruby /usr/local/bin/
RUN chmod a+x /usr/local/bin/yast-ci-ruby

# just a backward compatible symlink, should be removed in the future...
RUN ln -s yast-ci-ruby /usr/local/bin/yast-travis-ruby

ENV LC_ALL=en_US.UTF-8
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
