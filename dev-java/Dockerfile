FROM z9000/base-ubuntu:latest

USER root
ENV JAVA_HOME=/usr/lib/jvm/msopenjdk-current
ENV MAVEN_HOME=/opt/maven
ENV PATH="${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${PATH}"

# Default to UTF-8 file.encoding
ENV LANG=en_US.UTF-8

# Install Microsoft OpenJDK
RUN arch="$(dpkg --print-architecture)" \
	&& case "$arch" in \
		"amd64") \
			jdkUrl="https://aka.ms/download-jdk/microsoft-jdk-21-linux-x64.tar.gz"; \
			;; \
		"arm64") \
			jdkUrl="https://aka.ms/download-jdk/microsoft-jdk-21-linux-aarch64.tar.gz"; \
			;; \
		*) echo >&2 "error: unsupported architecture: '$arch'"; exit 1 ;; \
	esac \
	\
	&& wget --progress=dot:giga -O msopenjdk.tar.gz "${jdkUrl}" \
	&& wget --progress=dot:giga -O sha256sum.txt "${jdkUrl}.sha256sum.txt" \
	\
	&& sha256sumText=$(cat sha256sum.txt) \
	&& sha256=$(expr substr "${sha256sumText}" 1 64) \
	&& echo "${sha256} msopenjdk.tar.gz" | sha256sum --strict --check - \
	&& rm sha256sum.txt* \
	\
	&& mkdir -p "$JAVA_HOME" \
	&& tar --extract \
		--file msopenjdk.tar.gz \
		--directory "$JAVA_HOME" \
		--strip-components 1 \
		--no-same-owner \
	&& rm msopenjdk.tar.gz* \
	\
	&& ln -s ${JAVA_HOME} /docker-java-home \
	&& ln -s ${JAVA_HOME} /usr/local/openjdk-${TARGET_JAVA_VERSION} \
	&& wget --progress=dot:giga -O maven.tar.gz "https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz" \
	&& mkdir -p /opt/maven \
        && tar --extract \
                --file maven.tar.gz \
                --directory /opt/maven \
                --strip-components 1 \
                --no-same-owner \
        && rm maven.tar.gz* \
