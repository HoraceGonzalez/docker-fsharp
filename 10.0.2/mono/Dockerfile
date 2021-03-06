FROM debian:stretch-slim
LABEL maintainer "Dave Curylo <dave@curylo.org>, Steve Desmond <steve@stevedesmond.ca>"

ENV MONO_THREADS_PER_CPU 50
RUN MONO_VERSION=5.8.0.127 && \
    FSHARP_VERSION=10.0.2 && \
    FSHARP_BASENAME=fsharp-$FSHARP_VERSION && \
    FSHARP_ARCHIVE=$FSHARP_VERSION.tar.gz && \
    FSHARP_ARCHIVE_URL=https://github.com/fsharp/fsharp/archive/$FSHARP_VERSION.tar.gz && \
    export GNUPGHOME="$(mktemp -d)" && \
    apt-get update && apt-get --no-install-recommends install -y gnupg dirmngr && \
    apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    echo "deb https://download.mono-project.com/repo/debian stretch/snapshots/$MONO_VERSION main" | tee /etc/apt/sources.list.d/mono-official-stable.list && \
    apt-get install -y apt-transport-https && \
    apt-get update -y && \
    apt-get --no-install-recommends install -y pkg-config make nuget mono-devel msbuild ca-certificates-mono && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /tmp/src && \
    cd /tmp/src && \
    printf "namespace a { class b { public static void Main(string[] args) { new System.Net.WebClient().DownloadFile(\"%s\", \"%s\");}}}" $FSHARP_ARCHIVE_URL $FSHARP_ARCHIVE > download-fsharp.cs && \
    mcs download-fsharp.cs && mono download-fsharp.exe && rm download-fsharp.exe download-fsharp.cs && \
    tar xf $FSHARP_ARCHIVE && \
    cd $FSHARP_BASENAME && \
    make && \
    make install && \
    cd ~ && \
    rm -rf /tmp/src /tmp/NuGetScratch ~/.nuget ~/.config ~/.local "$GNUPGHOME" && \
    apt-get purge -y make gnupg dirmngr && \
    apt-get clean
WORKDIR /root
CMD ["fsharpi"]
