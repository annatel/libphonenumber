VERSION 0.5

alpine-base:
    FROM elixir:1.12.2-alpine
    RUN apk add --no-progress --update git build-base zip
    RUN apk --no-cache --update add libgcc libstdc++ \
        git make g++ \
        build-base gtest gtest-dev boost boost-dev protobuf protobuf-dev cmake icu icu-dev openssl \
        && \
        rm -rf /var/cache/apk/*

libphonenumber:
    FROM +alpine-base

    WORKDIR /libphonenumber
    COPY --dir . .
    
    WORKDIR /libphonenumber/cpp/build
    RUN mkdir ./assets
    RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=./assets ..
    RUN make install
    WORKDIR assets
    RUN zip -r ../assets.zip *

    SAVE ARTIFACT /libphonenumber/cpp/build/assets.zip AS LOCAL cpp/build/assets.zip

