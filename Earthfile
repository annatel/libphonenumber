VERSION 0.5

alpine-3.17:
  FROM --platform=$BUILDPLATFORM alpine:3.17
  RUN apk add --no-progress --update git build-base zip
  RUN apk --no-cache --update add libgcc libstdc++ \
      git make g++ \
      build-base gtest gtest-dev boost boost-dev protobuf protobuf-dev cmake icu icu-dev openssl \
      && \
      rm -rf /var/cache/apk/*
  RUN apk add openjdk8-jre

alpine-3.21:
  FROM --platform=$BUILDPLATFORM alpine:3.21
  RUN apk add --no-progress --update git build-base zip
  RUN apk --no-cache --update add libgcc libstdc++ \
      git make g++ \
      build-base gtest gtest-dev boost boost-dev protobuf protobuf-dev cmake icu icu-dev openssl \
      && \
      rm -rf /var/cache/apk/*
  RUN apk add openjdk8-jre

ubuntu-20.04:
  FROM --platform=$BUILDPLATFORM ubuntu:20.04
  RUN apt-get update && apt-get -y upgrade
  RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get -y install tzdata
  RUN apt-get -y install \
      build-essential pkg-config git zip hashdeep \
      cmake cmake-curses-gui libprotobuf-dev libgtest-dev libre2-dev \
      libicu-dev libboost-dev libboost-thread-dev libboost-system-dev openssl \
      protobuf-compiler openjdk-8-jre
  RUN rm -rf /var/cache/apt/*

ubuntu-22.04:
  FROM --platform=$BUILDPLATFORM ubuntu:22.04
  RUN apt-get update && apt-get -y upgrade
  RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get -y install tzdata
  RUN apt-get -y install \
      build-essential pkg-config git zip hashdeep \
      cmake cmake-curses-gui libprotobuf-dev libgtest-dev libre2-dev \
      libicu-dev libboost-dev libboost-thread-dev libboost-system-dev openssl \
      protobuf-compiler openjdk-8-jre
  RUN rm -rf /var/cache/apt/*

libphonenumber:
  ARG TARGETARCH
  ARG --required buildos

  FROM +${buildos}

  WORKDIR /libphonenumber
  COPY --dir . .

  # kludge: COPY creates files in the past causing make not rebuild some resources
  # RUN find . -print0 | xargs -0 touch
  RUN touch /libphonenumber/resources/PhoneNumberMetadata.xml

  WORKDIR /libphonenumber/cpp/build
  RUN mkdir assets
  RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=./assets ..
  RUN make install

  WORKDIR assets
  RUN rm -rf cmake
  RUN zip -r ../libphonenumber_${TARGETARCH}-${buildos}.zip *

  SAVE ARTIFACT /libphonenumber/cpp/build/libphonenumber_${TARGETARCH}-${buildos}.zip AS LOCAL cpp/build/libphonenumber_${TARGETARCH}-${buildos}.zip

libphonenumber-deb:
  ARG TARGETARCH
  ARG --required buildos
  ARG --required version

  FROM +${buildos}

  WORKDIR /deb
  COPY cpp/build/libphonenumber_*.zip .
  RUN unzip libphonenumber_*.zip -d phonenumbers

  WORKDIR /deb/phonenumbers
  RUN mkdir -p usr/local
  RUN mv include lib usr/local
  RUN mkdir DEBIAN
  RUN md5deep -lr usr > DEBIAN/md5sums
  RUN echo "\
Package: phonenumbers
Version: ${version}
Maintainer: annatel <support@annatel.net>
Architecture: ${TARGETARCH}
Section: libs
Description: Annatel phonenumbers library
  Annatel phonenumbers library with extended numbers
Depends: protobuf-compiler, libboost-thread-dev
" > DEBIAN/control

  WORKDIR /deb
  RUN fakeroot dpkg-deb --build phonenumbers

  SAVE ARTIFACT /deb/phonenumbers.deb AS LOCAL cpp/build/libphonenumber_${TARGETARCH}-${buildos}.deb
