FROM localhost/ceph/daemon:ablestack-allo-pacific-centos-8-x86_64

# Who is the maintainer ?
LABEL maintainer="ablecloud <dsavinea@redhat.com>"

# Is a ceph container ?
LABEL ceph="True"

# What is the actual release ? If not defined, this equals the git branch name
LABEL RELEASE="ablestack"

# What was the url of the git repository
LABEL GIT_REPO="https://github.com/kt-cheon/ablestack-storage.git"

# What was the git branch used to build this container
LABEL GIT_BRANCH="master"

# What was the commit ID of the current HEAD
LABEL GIT_COMMIT="607de43feb2a2d4626667a392bfe10377e9fb32d"

# Was the repository clean when building ?
LABEL GIT_CLEAN="False"

# What CEPH_POINT_RELEASE has been used ?
LABEL CEPH_POINT_RELEASE=""

#======================================================
# Install ceph and dependencies, and clean up
#======================================================
#======================================================
# Add ceph-container files
#======================================================

# Add s3cfg file
#ADD s3cfg /root/.s3cfg

# Add templates for confd
#ADD ./confd/templates/* /etc/confd/templates/
#ADD ./confd/conf.d/* /etc/confd/conf.d/

# Add bootstrap script, ceph defaults key/values for KV store
ADD *.sh check_zombie_mons.py ./osd_scenarios/* entrypoint.sh.in disabled_scenario /opt/ceph-container/bin/
#ADD ceph.defaults /opt/ceph-container/etc/
# ADD *.sh ceph.defaults check_zombie_mons.py ./osd_scenarios/* entrypoint.sh.in disabled_scenario /

# Copye sree web interface for cn
# We use COPY instead of ADD for tarball so that it does not get extracted automatically at build time
# COPY Sree-0.2.tar.gz /opt/ceph-container/tmp/sree.tar.gz
# ABLESTACK Branding

COPY ablestack-branding/ablestack_Logo_with_text_color_white.svg /usr/share/ceph/mgr/dashboard/frontend/dist/en-US/assets/Ceph_Ceph_Logo_with_text_red_white.svg
COPY ablestack-branding/ablestack_Logo_with_text_white.svg /usr/share/ceph/mgr/dashboard/frontend/dist/en-US/assets/Ceph_Ceph_Logo_with_text_white.svg
COPY ablestack-branding/ablestack_cloud_Logo.svg /usr/share/ceph/mgr/dashboard/frontend/dist/en-US/assets/Ceph_Logo.svg
COPY ablestack-branding/index.html /usr/share/ceph/mgr/dashboard/frontend/dist/en-US/

# Modify the entrypoint
RUN bash "/opt/ceph-container/bin/generate_entrypoint.sh" && \
  rm -f /opt/ceph-container/bin/generate_entrypoint.sh && \
  bash -n /opt/ceph-container/bin/*.sh

# Execute the entrypoint
WORKDIR /
ENTRYPOINT ["/opt/ceph-container/bin/entrypoint.sh"]
