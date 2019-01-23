FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive
ENV POCO_DOWNLOAD_URL https://github.com/pocoproject/poco/archive/poco-1.9.0-release.tar.gz
ENV POCO_DOWNLOAD_SHA256 45ec5c759d2ad02bb812745f6cb585aa04af025c921b1bb4bdefa108ce4f1756

RUN apt-get update -qq \
	&& apt-get install -yq software-properties-common python-software-properties \
	&& apt-add-repository ppa:ubuntu-toolchain-r/test \
	&& apt-get update \
	# Install CMake 3.7.2, GCC/G++ 6 and other dependencies
	&& apt-get install -yq --no-install-recommends \
		curl \
		git \
		libssl-dev \
		apt-utils \
		build-essential \
		unixodbc \ 
		unixodbc-dev \
		ca-certificates \
		libncurses-dev \
		libmysqlclient-dev \
		pkg-config \
		apache2 \
		apache2-dev \
		cmake \
		gcc-6 \
		g++-6 \
	&& update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 60 --slave /usr/bin/g++ g++ /usr/bin/g++-6

# Install Poco C++ Libraries 1.9.0
#	RUN curl -fsSL "${POCO_DOWNLOAD_URL}" -o /tmp/poco.tar.gz \
#	&& echo "$POCO_DOWNLOAD_SHA256  /tmp/poco.tar.gz" | sha256sum -c - \
#	&& tar --directory /tmp -xzf /tmp/poco.tar.gz \
#	&& cd /tmp/poco-poco-1.9.0-release && cmake . \
#		-DENABLE_DATA_ODBC=ON \
#		-DENABLE_DATA_MYSQL=ON \
#		-DENABLE_PAGECOMPILER=OFF \
#		-DENABLE_MONGODB=ON \
#		-DENABLE_PAGECOMPILER=OFF \
#		-DENABLE_APACHECONNECTOR=ON \
#		-DENABLE_TESTS=OFF \
#	&& make && make install \
#	&& rm /tmp/poco.tar.gz

RUN git clone https://github.com/edson-a-soares/poco.git  /tmp/poco \
	&& cd /tmp/poco && cmake . \
	&& make && make install \
	&& cp -p /tmp/poco/lib/libmod_poco.so /usr/lib/apache2/modules/mod_poco.so
	# && rm /tmp/poco

ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

CMD ["/usr/sbin/apachectl", "-DFOREGROUND"]
