FROM debian:stretch-slim

# /var/lib/dpkg/info/plexmediaserver.postinst: 39: /var/lib/dpkg/info/plexmediaserver.postinst: start: not found
RUN ln -s /bin/true /usr/local/bin/start

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		ca-certificates \
		wget \
	&& rm -rf /var/lib/apt/lists/*

# https://www.plex.tv/downloads/#getdownload
# https://plex.tv/api/downloads/1.json
ENV PLEX_VERSION 1.11.3.4803-c40bba82e
ENV PLEX_URL https://downloads.plex.tv/plex-media-server/1.11.3.4803-c40bba82e/plexmediaserver_1.11.3.4803-c40bba82e_amd64.deb
ENV PLEX_SHA1 c3d4b6fc44340304b8fa95b682e17c4bac96a00f

RUN set -eux; \
	wget -O /tmp/plex.deb "$PLEX_URL"; \
	echo "$PLEX_SHA1 */tmp/plex.deb" | sha1sum -c -; \
	apt-get update; \
	apt-get install /tmp/plex.deb; \
	rm -rf /var/lib/apt/lists/*; \
	rm /tmp/plex.deb

# https://github.com/plexinc/pms-docker/blob/7529befa25c89a67731129f71ee73c22a3a647d6/root/etc/services.d/plex/run
# https://support.plex.tv/articles/200273978-linux-user-and-storage-configuration/
# https://github.com/plexinc/PlexMediaServer-Ubuntu/blob/1c48f4aa97a223143ec6a7bdbbb7942a15dc70cf/debian/start_pms
ENV PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR /var/lib/plexmediaserver/Library/Application Support
ENV PLEX_MEDIA_SERVER_HOME /usr/lib/plexmediaserver
ENV PLEX_MEDIA_SERVER_INFO_DEVICE docker
ENV PLEX_MEDIA_SERVER_MAX_PLUGIN_PROCS 6
ENV PLEX_MEDIA_SERVER_TMPDIR /tmp
ENV LD_LIBRARY_PATH /usr/lib/plexmediaserver
ENV LANG C.UTF-8

RUN mkdir -p "$PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR" && chmod -R a+rwX "$PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR"
VOLUME $PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR

COPY docker-entrypoint.sh /

EXPOSE 32400
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["/usr/lib/plexmediaserver/Plex Media Server"]
